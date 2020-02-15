---
title: "The Per Rewrite Diary: Day 15"
date: 2020-02-15T08:45:00-05:00
draft: false
summary: A series on the long-overdue rewrite of my iOS app Per. Today, I try refactoring some of product detail view into its own class.
tags: ["per", "per rewrite diary", "ios"]

---

> This post is part of a series about rewriting my iOS app, [Per]. Per is a price per unit comparison app with a bunch of neat convenience figures, but it hasn't been updated in years, so I'm rewriting it from scratch to eliminate a bunch of technical debt. Just because it's not an open-source app doesn't mean I can't share what I learn as I go!
> 
> See the rest of the series [here].

## Subclassing UIViews

[Yesterday] I had the `ProductDetailContentViewController` working (more or less), but I was unhappy with just how big it was getting. It's got two main components: a form in which the user enters product details (price, quantity, units), and a pair of buttons to add the product to the list, or cancel the action altogether.

Today, I started work on subclassing `UIView` to move that form component into its own `ProductDetailFormView`, to pull all of those controls (three `UITextField`s, two `UIStackView`s, and a ~~partridge in a pear tree~~ `UILabel`) and the form's own layout into its own class.

Once again, Frank Courville's got [a handy article] for this! So far, I've started writing the `setupView()` and `setupConstraints()` methods for the class. One little change that I like is to declare my controls as `lazy`, so that I don't have to worry about unwrapping optionals (force-unwrap and `guard let` both feel like the wrong way to reason about views, and [John Sundell agrees]):

```
class ProductDetailFormView: UIView {
    lazy var quantityTextField = UITextField()
    lazy var unitsTextField = UITextField()
    lazy var measurementStackView = UIStackView()
    lazy var forLabel = UILabel()
    lazy var priceTextField = UITextField()
    lazy var formStackView = UIStackView()

    override init(frame: CGRect) {
        super.init(frame: frame)
        
        setupView()
        setupConstraints()
    }
    
    required init?(coder: NSCoder) {
        super.init(coder: coder)
        
        setupView()
        setupConstraints()
    }
    
    // Create the views and define some basic styling.
    func setupView() {
        quantityTextField.placeholder = "0"
        quantityTextField.textAlignment = .right
        quantityTextField.keyboardType = .decimalPad
        quantityTextField.borderStyle = .roundedRect
        
        unitsTextField.placeholder = "units"
        unitsTextField.textAlignment = .center
        unitsTextField.isEnabled = false          // Deal with units later
        unitsTextField.borderStyle = .roundedRect
        
        measurementStackView.axis = .horizontal
        measurementStackView.distribution = .fillEqually
        measurementStackView.alignment = .center
        measurementStackView.spacing = 16
        
        measurementStackView.addArrangedSubview(quantityTextField)
        measurementStackView.addArrangedSubview(unitsTextField)
        
        forLabel.text = "for"
        forLabel.textAlignment = .right
        
        priceTextField.placeholder = "0.00"
        priceTextField.textAlignment = .right
        priceTextField.keyboardType = .decimalPad
        priceTextField.borderStyle = .roundedRect
        
        formStackView.axis = .vertical
        formStackView.distribution = .equalSpacing
        formStackView.alignment = .fill
        formStackView.spacing = 16
        
        formStackView.addArrangedSubview(measurementStackView)
        formStackView.addArrangedSubview(forLabel)
        formStackView.addArrangedSubview(priceTextField)
        
        addSubview(formStackView)
    }

    func setupConstraints() {
        formStackView.translatesAutoresizingMaskIntoConstraints = false
        
        NSLayoutConstraint.activate([
            formStackView.topAnchor.constraint(equalTo: layoutMarginsGuide.topAnchor, constant: 16),
            formStackView.leadingAnchor.constraint(equalTo: layoutMarginsGuide.leadingAnchor, constant: 16),
            formStackView.trailingAnchor.constraint(equalTo: layoutMarginsGuide.trailingAnchor, constant: -16)
        ])
    }
}
```

I'm running into some trouble figuring out the constraints right now. Specifically, the form take the full width of the top of the superview, and its height should be the height of its contents (the `formStackView`). That height is where I'm a bit stuck, because when I call the `ProductDetailFormView` initializer from its superview, I have to hand it a `frame`; it's clear to me that the frame's origin is `(0, 0)` and that its width would be `view.bounds.width`, but I'm haven't quite figured out the best way to set its height.

I could give it a third of the height of the superview (`view.bounds.height / 3`), but that's not adaptive, so if I want to change, say, font sizes in the form view, I need to ensure that it still fits whatever portion of the superview height. That's silly.

In the form view's initializer, I could throw away whatever frame height I get, but it's still not clear to me how I get the form's inherent height _in the initializer_. In Frank's article, he calls it with a `.zero` frame (i.e., at the origin, with zero size); if I do that, then I get a broken layout and a warning:

```
[LayoutConstraints] Unable to simultaneously satisfy constraints.
	Probably at least one of the constraints in the following list is one you don't want. 
	Try this: 
		(1) look at each constraint and try to figure out which you don't expect; 
		(2) find the code that added the unwanted constraint or constraints and fix it. 
	(Note: If you're seeing NSAutoresizingMaskLayoutConstraints that you don't understand, refer to the documentation for the UIView property translatesAutoresizingMaskIntoConstraints) 
```

If I set

```
productDetailFormView.translatesAutoresizingMaskIntoConstraints = false
```

then the warning disappears, and the layout kind-of shows up, but it's sized entirely according to the intrinsic size of the form's controls, not the full width of the superview, and trying to anchor other things to the form view's anchors doesn't work properly because it's still got that `.zero` frame.

This is where my relative inexperience with writing custom `UIView`s is throwing me for a loop, but I'll dig into this a bit more tomorrow!

[Per]: https://droppedbits.com/apps/per
[here]: /tags/per-rewrite-diary/
[Yesterday]: /post/per-diaries-day-14/
[a handy article]: https://ioscoachfrank.com/uiview-basics.html
[John Sundell agrees]: https://swiftbysundell.com/articles/handling-non-optional-optionals-in-swift/