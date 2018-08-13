+++
date = "2016-07-15T12:00:00-04:00"
title = "Using XCTAssertThrowsError in your Swift tests"

+++

There's a new kid on the XCTest block, and its name is [XCTAssertThrowsError](https://developer.apple.com/reference/xctest/1500795-xctassertthrowserror).

I haven't been able to find much on its usage aside from its original discussion on the [swift-evolution mailing list](https://lists.swift.org/pipermail/swift-evolution/Week-of-Mon-20160104/006091.html) and a [Stack Overflow question](http://stackoverflow.com/questions/36190161/xctassertthrowserror-strange-behavior-with-custom-errorhandler), so here's a little bit of a discussion on how I'm using it in a new project of mine.

Swift introduced some pretty neat error handling in 2.0, and [Natasha the Robot provided a nice guide](https://www.natashatherobot.com/swift-2-error-handling/) on how to throw an error in your code.

So, as a contrived example, let's say you have a class called `AccountManager` that manages a set of `Account` objects:

	enum ListError: ErrorType {
		case AccountAlreadyExistsInList
		case AccountDoesNotExistInList
	}
		
	class AccountManager {
		var accountList = Set<Account>()    // Account conforms to Hashable, Equatable. I promise.
		
		func add(_ account: Account) throws {
			if (accountList.contains(account)) {
				throw ListError.AccountAlreadyExistsInList
			}
			else {
				accountList.insert(account)
			}
		}

		func remove(_ account: Account) throws {
			if (accountList.contains(account)) {
				accountList.remove(account)
			}
			else {
				throw ListError.AccountDoesNotExistInList
			}
		}
	}

(Note that in Swift 3, `ErrorType` has been renamed to [`ErrorProtocol`](https://developer.apple.com/reference/swift/errorprotocol).)

A couple of things to know about the [Set](https://developer.apple.com/reference/swift/set) type:

- No duplicates can be added, a Set is silent when you try to add an element that it already contains.
- Unless you're checking its return type, Set is also silent when you try to remove an element that it doesn't contain<sup id="fnref:1"><a href="#fn:1" rel="footnote">1</a></sup>.

While this protects the integrity of the Set, it could be a bit frustrating for consumers of the `AccountManager` class, because there's no way to surface what's going on when we try to add a duplicate or remove a non-existent element. So, we throw!

Specifically, what we're doing in the `AccountManager` class is checking to see if the `Account` argument we're passing to the `add(account:)` and `remove(account:)` functions already exists in the `accountList` Set, and handling the result appropriately:

1. If we're trying to `remove` an `Account` from the `accountList` and it exists, go ahead and do so. If it doesn't, throw `ListError.AccountDoesNotExistInList`.
2. If we're trying to `add` an `Account` to the `accountList` and it exists, throw `ListError.AccountAlreadyExistsInList`. If it doesn't, go ahead and add it.

And of course, in our unit tests, we want to check that these errors are thrown properly. Enter `XCTAssertThrowsError`!

    // Test that adding a duplicate account to an accountList throws an error.
    func testAdd_AddingDuplicateAccount_ThrowsAccountAlreadyExistsInList() {
        let accounts = AccountManager()
        let firstAccount = Account(descriptiveName: "Account 1", accountNumber: "12345AZ")
        let secondAccount = Account(descriptiveName: "Account 1", accountNumber: "12345AZ")

        accounts.add(firstAccount)

        XCTAssertThrowsError(try accounts.add(secondAccount))
    }

    // Test that removing an account from an empty accountList throws an error.
    func testRemove_RemovingAccountFromEmptyList_ThrowsAccountDoesNotExistInList() {
        let accounts = AccountManager()
        let firstAccount = Account(descriptiveName: "Account 1", accountNumber: "12345AZ")
        
        XCTAssertThrowsError(try accounts.remove(firstAccount))
    }

Run the tests and they'll pass, because the tested expression throws an error. Cool!

This is a good start, but we're only testing that _an_ error is thrown. That's not good enough, of course, because _any_ error thrown will make this test pass, but we're looking for a _specific_ error. Let's take a closer look at the declaration for `XCTAssertThrowsError`:

    func XCTAssertThrowsError<T>(_ expression: @autoclosure () throws -> T,
                                    _ message: @autoclosure () -> String = default,
                                         file: StaticString = #file,
                                         line: UInt = #line,
                                 errorHandler: (error: ErrorProtocol) -> Void = default)

I've split the signature up so that there's just one argument per line. The description for each argument is [available in the  documentation](https://developer.apple.com/reference/xctest/1500795-xctassertthrowserror#parameters), so I won't repeat them here, but the important thing to note is that `XCTAssertThrowsError` is actually a [generic](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Generics.html) on `T`. This means that in the `expression` argument, we can add a closure that checks to see if, in fact, an error of type `T` is being thrown.

So let's add those checks to our two tests:

    // Test that adding a duplicate account to an accountList throws an AccountAlreadyExistsInList error.
        func testAdd_AddingDuplicateAccount_ThrowsAccountAlreadyExistsInList() {
        let accounts = AccountManager()
        let firstAccount = Account(descriptiveName: "Account 1", accountNumber: "12345AZ")
        let secondAccount = Account(descriptiveName: "Account 1", accountNumber: "12345AZ")
        
        accounts.add(firstAccount)
        
        XCTAssertThrowsError(try accounts.add(secondAccount)) { (error) -> Void in
        	XCTAssertEqual(error as? ListError, ListError.AccountAlreadyExistsInList)
        }
    }
    
    // Test that removing an account from an empty accountList throws an AccountDoesNotExistInList error.
    func testRemove_RemovingAccountFromEmptyList_ThrowsAccountDoesNotExistInList() {
        let accounts = AccountManager()
        let firstAccount = Account(descriptiveName: "Account 1", accountNumber: "12345AZ")
    
        XCTAssertThrowsError(try accounts.remove(firstAccount)) { (error) -> Void in
        	XCTAssertEqual(error as? ListError, ListError.AccountDoesNotExistInList)
        }
    }

Now we're _sure_ that we're testing that the right error is being thrown in our tests: in the closure, we call _another_ assertion, `XCTAssertEqual`, to check that the error being thrown is the type of `ListError` that we expect.

What does this mean? We no longer have to create weirdo functions that return a tuple `<U, V>`, where `U` is the result and `V` is an error that you can check for.

You can add other arguments to your assertion, like a message or the specific file and line number if the test fails, but for now this should be enough to get you started checking your error throwing.

<div class="footnotes">
  <ol>
  	<li class="footnote" id="fn:1">
  	<p>Of course, you <i>should</i> be checking the return type, and you’d see that you got back <code>nil</code>, but like I said: this is a contrived example.<a href="#fnref:1" title="return to article"> ↩</a></p>
	</li>
  </ol>
</div>