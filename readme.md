# android-sdk

## Introduction
Our SDK provides a wide range of both utility and client-facing features that will save time and make development easier than ever with APS Card. It's built purely on Apple Core Location and Core Bluetooth technologies and is 100% iBeacon compatible.
## Requirements
* Android 18+
* Android Studio
## Installation
### JCenter
Add ```maven { url 'https://lockcardlib.bintray.com/maven' }``` to build.gradle(Module: app) -> repositories and Add ```compile 'com.friver.apscard:lockcardlib:1.0@aar'``` to dependencies. More on [Android dependencies here](https://developer.android.com/studio/build/dependencies.html).
### Manual
APS Card SDK comes to you as a aar. To setup, you only need to include a single lockcardlib.arr. arr file in your project to get started:
1. Drag and drop APSCardSDK.framework file into your Xcode project. It will automatically show up in your project navigator and will be added to "Linked Frameworks and Libraries" section in project settings.
2. APSCard SDK depends on Apple's CoreLocation and CoreBlueooth frameworks as well as SystemConfiguration framework to handle APSCard Server API requests, so you should include them in your project too. When you add them to your project settings, it should look like on the screenshot below.
## Usage
### Configuration
### Initilization
0. ApsCardLib 클래스는 application 을 상속받습니다.
1. 사용자 어플리케이션에서 ApsCardLib 를 상속받습니다.
2. mScanResultInterface를 선언해줍니다.(아래 Callback 의 2번 참고)
3. Application의 onCreate 함수에서 ```super.setScanResultInterface(mScanResultInterface);``` 를 호출해줍니다.
4. 첫 진입 IntroActivity 에서 ```ApsCardLib.getInstance().initBle();``` 를 호출해줍니다.

### Callback

1. LeScanCallback
  안드로이드 BluetoothAdapter 의 LeScanCallback 입니다.
  <pre><code>
  private BluetoothAdapter.LeScanCallback mLeScanCallback =
        new BluetoothAdapter.LeScanCallback() {
        @Override
        public void onLeScan(final BluetoothDevice device, int rssi, byte[] scanRecord) {
        }
    };
  </code></pre>

2. ScanResultInterface
  백그라운드에서 카드 검색이 되었을 때 콜백받는 인터페이스입니다.
<pre><code>private ScanResultInterface mScanResultInterface = new ScanResultInterface() {
        @Override
        public void onCardScan(String macAddress) {
        }
    };</code></pre>

3. ICardListener
  카드 등록, 언락 등 카드와 연동하고 난 결과와 과정을 받는 콜백입니다.
  <pre><code>
    private ICardListener iCardListener = new ICardListener(){

        @Override
        public void onSuccess(int reqType) {
            ApsCardLib.getInstance().startBackgroundScan(); //일반적으로 이 곳에서 startBackgroundScan를 호출해줍니다.(아래 APIS 의 startBackgroundScan를 참고)
        }

        @Override
        public void onFail(String error) {
        }

        @Override
        public void onUpdateProgress(int progress, int part, int total) {

        }

        @Override
        public void onUpdateCardSynchProgress(int progress, int part, int total) {

        }
        @Override
        public void onConnectStatus(int status) {
            switch(status) {
                case BluetoothProfile.STATE_CONNECTING:
                    break;

                case BluetoothProfile.STATE_CONNECTED:
                    break;

                case BluetoothProfile.STATE_DISCONNECTED:

                    break;
            }
        }

        @Override
        public void onBatteryLevel(String macAddress, int batteryLevel) {

        }
    };
    </pre></code>
### APIs

- public void setScanResultInterface(ScanResultInterface scanResultInterface)
  ApsCardLib 를 상속받는 Application의 conCreate 함수에서 호출해줍니다.
  ScanResultInterface 는 백그라운드 스캔 시 콜백 받는 인터페이스입니다.

- public void initBle()
  Card 연동을 위한 Beacon 관련 기능 및 내부 데이터 초기화

- public void startScanCard(final BluetoothAdapter.LeScanCallback leScanCallback)
  1초 뒤부터 카드 스캔을 시작하고, 카드 검색이 되면 leScanCallback로 넘겨줍니다.
  이 함수를 호출하면 내부적으로 백그라운드 스캔을 멈춥니다.

- public void stopScanCard(BluetoothAdapter.LeScanCallback leScanCallback)
  사용자 앱에서 카드 스캔을 끝낼 때 호출합니다. 이 함수를 호출하면 내부적으로 백그라운드 스캔을 다시 시작합니다.

- public void startBackgroundScan()
  unlock 이나 카드 등록을 하면 백그라운드 스캔이 중지되어서, 이것들이 끝난 후에 호출해줍니다.

- public void registerCard(final ICardListener iCardListener, final String macAddress, final String password, final String authNum, final String phoneNum)
  카드를 등록합니다. 등록 과정과 결과를 iCardListener 로 보내줍니다.

- public void deleteCard(String macAddress)
  사용자 앱에서 등록 한 카드를 삭제할 때 호출해주면 library 에 저장되어있는 카드 정보도 삭제해줍니다.

- public void unlockCard(ICardListener iCardListener,String macAddress)
  카드를 언락합니다. 일반적으로 언락 처리 후 startBackgroundScan 을 호출해줍니다.

- public void changePassword(ICardListener iCardListener,String macAddress,String newPassword)
  카드의 비밀번호를 변경해줍니다.(아직 테스트x)

- public boolean checkPassword(String macAddress,String password)
  카드의 비밀번호를 확인 후 결과를 리턴해줍니다.

- public boolean checkAuthNum(String authNum)
  등록되어 있는 카드 중 authNum 이 일치하는게 있는지 결과를 리턴해줍니다.
