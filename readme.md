# ios-sdk

## Introduction
Our SDK provides a wide range of both utility and client-facing features that will save time and make development easier than ever with APS Card. It's built purely on Apple Core Location and Core Bluetooth technologies and is 100% iBeacon compatible.
## Requirements
* iOS 8.0+
* Xcode 8
## Installation
### CocosPods
Add ```pod APSCardSDK``` to your Podfile and run ```pod install```. More on [CocoaPods here](http://cocospods.com).
### Manual
APS Card SDK comes to you as a framework. To setup, you only need to include a single APSCardSDK.framework file in your project to get started:
1. Drag and drop APSCardSDK.framework file into your Xcode project. It will automatically show up in your project navigator and will be added to "Linked Frameworks and Libraries" section in project settings.
2. APSCard SDK depends on Apple's CoreLocation and CoreBlueooth frameworks as well as SystemConfiguration framework to handle APSCard Server API requests, so you should include them in your project too. When you add them to your project settings, it should look like on the screenshot below.
## Usage
### Configuration
### Initilization
<pre><code>
</code></pre>
### APIs
- delegate 설정 및 callback 함수..

- public func initialize()
  Card 연동을 위한 Beacon 관련 기능 및 내부 데이터 초기화

- public func getCardList()
  SCAN한 카드 목록을 반환

- func startRegitration()
  해당 카드를 APP에 등록한다.

- public func unlockCard()
  해당 카드의 잠금을 해제한다.

- public func changePassword()
  해당 카드의 패스워드를 변경한다.

- public func matchPassword()
  카드 Unlock Password 일치여부 확인(내부 DB로 확인, 카드 연동 불필요)

- public func matchAuthNo()
  APP에 등록된 카드 중에 해당 Auth No를 가진 카드가 있는지 확인

- public func didReceiveNoti(_ application: UIApplication, didReceive notification: UILocalNotification)
  AppDelegate 내 didReceive func에 추가
  (검색된 Beacon에 대한 정보 확인)

- public func cancelCurrentPeriphiral() - Temporary Use. Card 정책과 충돌이 날 수 있음.
  현재 active 또는 pending 상태의 연결을 취소함. Non-blocking으로 동작. 해당 명령 후에도 연결이 pending 상태로 남아있을 수 있음.
  
