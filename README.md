# UPI India 

UPI India is a versatile Flutter plugin designed to seamlessly facilitate UPI transactions across various Android apps, making it easier than ever to handle digital payments in your Flutter application.

## Features

- Retrieve a list of UPI apps installed on the device.
- Start UPI transactions with ease.
- Handle various UPI payment statuses and exceptions gracefully.

## Installation

Add the UPI India plugin to your `pubspec.yaml` file:

```yaml
dependencies:
  upi_india: ^1.0.0
```

## Usage

### Classes

1. **UpiIndia** - The main class containing two essential methods:

   - `getAllUpiApps()` - Retrieves a list of UPI apps installed on the device.
   
   - `startTransaction()` - Initiates a UPI transaction.

2. **UpiApp** - Represents supported UPI apps and serves as a model class for the apps returned by `getAllUpiApps()`.

3. **UpiResponse** - Use this class to capture responses from the requested UPI app.

4. **UpiPaymentStatus** - Check the status of UPI payments with this class.

### Exceptions

Handle exceptions gracefully:

1. **UpiIndiaAppNotInstalledException** - Raised when the requested UPI app is not installed on the device.

2. **UpiIndiaUserCancelledException** - Raised when the user cancels the transaction.

3. **UpiIndiaNullResponseException** - Raised when the requested UPI app doesn't return any response.

4. **UpiIndiaInvalidParametersException** - Raised when the requested UPI app cannot handle the transaction.

5. **UpiIndiaActivityMissingException** - Raised for missing activity during the transaction.

6. **UpiIndiaAppsGetException** - Raised for errors while retrieving UPI apps.

### How to Start a Transaction?

Follow these steps to initiate a UPI transaction in your Flutter app:

#### Step 1: Import the Package

Import the UPI India package in your Dart code:

```dart
import 'package:upi_india/upi_india.dart';
```

#### Step 2: Create an UpiIndia Object

Instantiate an `UpiIndia` object:

```dart
UpiIndia _upiIndia = UpiIndia();
```

#### Step 3: Get the List of Supported UPI Apps

Retrieve a list of UPI apps installed on the device:

```dart
List<UpiApp>? apps;

@override
void initState() {
  _upiIndia.getAllUpiApps(mandatoryTransactionId: false).then((value) {
    setState(() {
      apps = value;
    });
  }).catchError((e) {
    apps = [];
  });
  super.initState();
}
```

Alternatively, you can directly use predefined apps like this:

```dart
UpiApp app = UpiApp.googlePay;
```

Assign the desired UPI app to the `app` parameter in Step 4.

#### Step 4: Create a Transaction Method

Create a method to start the transaction when called:

```dart
Future<UpiResponse> initiateTransaction(UpiApp app) async {
  return _upiIndia.startTransaction(
    app: app,
    receiverUpiId: "receiver@example.com",
    receiverName: 'John Doe',
    transactionRefId: 'Transaction123',
    transactionNote: 'Payment for goods',
    amount: 100.00,
  );
}
```

#### Step 5: Call the Transaction Method

Trigger the transaction method on a button click or using a `FutureBuilder`, and handle the response.

### How to Handle Response?

After receiving the snapshot from the Future, check for errors:

```dart
if (snapshot.hasError) {
  switch (snapshot.error.runtimeType) {
    case UpiIndiaAppNotInstalledException:
      print("Requested app not installed on device");
      break;
    case UpiIndiaUserCancelledException:
      print("You cancelled the transaction");
      break;
    case UpiIndiaNullResponseException:
      print("Requested app didn't return any response");
      break;
    case UpiIndiaInvalidParametersException:
      print("Requested app cannot handle the transaction");
      break;
    default:
      print("An unknown error has occurred");
      break;
  }
}
```

If `snapshot.hasError` is false, access the response `UpiResponse` from `snapshot.data` and extract relevant information such as:

- Transaction ID
- Response Code
- Approval Reference Number
- Transaction Reference ID
- Status

### Supported Apps

#### Verified Apps (Included by Default)

These apps have been tested and are verified to work with this plugin:

- All Bank
- Amazon Pay
- Axis Pay
- Baroda Pay
- BHIM
- Cent UPI
- Cointab
- Corp UPI
- DCB UPI
- Fino BPay
- Freecharge
- Google Pay
- iMobile ICICI
- Indus Pay
- Khaali Jeb
- Maha UPI
- Mobikwik
- Oriental Pay
- Paytm
- Paywiz
- PhonePe
- PSB
- SBI Pay
- Yes Pay

#### Apps That Don't Return Transaction ID

Some apps do not return a Transaction ID in their response. To use them, pass `mandatoryTransactionId: false` when calling `getAllUpiApps()`:

- MiPay (Both Play Store and GetApps version)
- HSBC Simply Pay

#### Non-Verified Apps

These apps haven't been tested yet, but you can use them by passing `allowNonVerifiedApps: true` when calling `getAllUpiApps()`:

- True Caller
- BOI UPI
- CSB UPI
- CUB UPI
- digibank
- Equitas UPI
- Kotak
- PayZapp
- PNB
- RBL Pay
- realme PaySa
- United UPI Pay
- Vijaya UPI

### Unsupported Apps

These apps are currently not working as expected:

- Airtel Thanks
- AUPay
- Bandhan Bank UPI
- CANDI
- Indian Bank UPI
- Jet Pay
- KBL UPI
- KVB UPI
- LVB UPAAY
- Synd UPI
- UCO UPI
- Ultra Cash
