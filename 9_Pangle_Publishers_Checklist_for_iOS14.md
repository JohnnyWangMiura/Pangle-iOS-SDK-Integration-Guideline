# 9. Pangle Publishers Checklist for iOS 14

1. Update apps to run on `Xcode 12.0 and higher version` to ensure compatibility with iOS 14.
2. Update to `Pangle iOS SDK 3.3.6.2` or `Pangle iOS SDK 3.4.1.1` and higher version for iOS 14 and SKAdNetwork Support
3. Add Pangle SKAdNetwork ID to your app's Info.plist to enable conversion tracking

```javascript
<key>SKAdNetworkItems</key>
  <array>
    <dict>
      <key>SKAdNetworkIdentifier</key>
      <string>238da6jt44.skadnetwork</string>
    </dict>
    <dict>
      <key>SKAdNetworkIdentifier</key>
      <string>22mmun2rn5.skadnetwork</string>
    </dict>
  </array>
```


4. Support for Apple’s App Tracking Transparency: Starting with iOS 14, IDFA will not be available until App Tracking Transparency calls for Tracking authorization requests from users. If the application does not request this, the IDFA obtained by the application will be automatically zeroed out, which may result in a reduction in your advertising revenue
  - To display the App Tracking Transparency authorization request for accessing the IDFA, update your Info.plist to add the NSUserTrackingUsageDescription key with a custom message describing your usage.
  - Below is an example description text:

    ```javascript
    <key>NSUserTrackingUsageDescription</key>
    <string>This identifier will be used to deliver personalized ads to you</string>
    ```

    - To display the authorization request prompt, call `requestTrackingAuthorization(completionHandler:)`` . We recommend waiting for the end user's authorization before loading ads,in order to get the user's authorized accurately.

Example for Swift

```swift
import AppTrackingTransparency
import AdSupport
...
func requestIDFA() {
  ATTrackingManager.requestTrackingAuthorization(completionHandler: { status in
    // Tracking authorization completed. Start loading ads here.
    // loadAd()
  })
}
```

Example for Objective-C

```objective-c
#import <AppTrackingTransparency/AppTrackingTransparency.h>
#import <AdSupport/AdSupport.h>
...
- (void)requestIDFA {
  [ATTrackingManager requestTrackingAuthorizationWithCompletionHandler:^(ATTrackingManagerAuthorizationStatus status) {
    // Tracking authorization completed. Start loading ads here.
    // [self loadAd];
  }];
}
```


## Note：
- App Tracking Transparency (ATT) is used to request user authorization to access app-related data for tracking the user or the device. Visit https://developer.apple.com/documentation/apptrackingtransparency for more information.
- SKAdNetwork (SKAN) is Apple's attribution solution that helps advertisers measure the success of ad campaigns while maintaining user privacy. Using Apple's SKAN, ad networks can attribute app installs even when IDFA is unavailable. Visit https://developer.apple.com/documentation/storekit/skadnetwork for more information.
- Before Apple requires developers to configure ATT, developers shouldn't configure ATT, which will affect the acquisition of IDFA, thus affecting the revenue.

