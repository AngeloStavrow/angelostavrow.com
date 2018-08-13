+++
date = "2016-07-22T12:00:00-04:00"
draft = false
title = "Wallet-based business cards"

+++

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Fun little morning project. 📇📲<br><br>(Barcode is blurred because I’m not quite done yet. 🙃) <a href="https://t.co/4eXh5NdTCP">pic.twitter.com/4eXh5NdTCP</a></p>&mdash; Angelo Stavrow (@AngeloStavrow) <a href="https://twitter.com/AngeloStavrow/status/731444850118279168">May 14, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

A couple of months ago I played a little bit with PassKit and Wallet after finding [this little tutorial](http://www.atomicbird.com/blog/passbook-card-details) on adding a business card to Passbook (now named Wallet).

A Wallet pass is pretty easy to create&mdash;just fill some metadata into a JSON file, create a Pass Type ID certificate in your Apple Developer account, and then run a signing utility. The whole process is pretty well described, step-by-step, [here](http://www.myuiviews.com/2014/06/01/step-by-step-create-a-passbook-business-card.html), so I won't re-iterate, but here are a couple of little perils and pitfalls to watch out for:

- To run `signpass`, you'll need to [download the Xcode project](https://developer.apple.com/services-account/download?path=/iOS/Wallet_Support_Materials/WalletCompanionFiles.zip) in the [Wallet Developer Guide](https://developer.apple.com/library/prerelease/content/documentation/UserExperience/Conceptual/PassKit_PG/). Unzip the download and open **signpass.xcodeproj** in the **signpass** folder. Build it, right-click on the executable in the **Products** folder in Xcode's file navigator, and select "Show in Finder"; drag and drop it into your Documents folder.

- After you run `signpass` from the Terminal, run `open PassName.pkpass` and it'll open in a Preview QuickLook window. From here, you can click on the "Add to Wallet" button and it'll go up to iCloud and download to your iOS devices, without having to upload it to a server.

![Preview QuickLook](/images/2016-07-22/pkpass-open-front.png)

- If the pass doesn't show up in your iOS device, there's probably a typo in your JSON. Look for extra/missing commas, parentheses, square brackets, &cet.

- You can double-check by launching Xcode's Simulator, launching the Wallet app, and dragging and dropping your .pkpass file from the Finder into your Simulator Wallet. If everything checks out, it'll ask if you want to add it.

![Simulator add pass](/images/2016-07-22/add-to-simulator.png)

- If you're uploading it to S3, make sure you set the Content-Type to `application/vnd.apple.pkpass`. Normally, this is a dropdown, but you can also just type whatever you want in there and S3 will accept it.

![S3 Content-Type](/images/2016-07-22/set-content-type.png)

And now you have a fancy electronic business card that other iPhone users can scan and download. Have fun!