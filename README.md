# AppsBunches Integration Requirements for Third-Party Services (Zid Merchants)

This document is a guideline for third-party providers who want to integrate their services into [Apps Bunches Mobile App for Zid merchants](https://apps.zid.sa/application/421).

### Notes To Consider

* Coordinating with the Partnerships Team regarding the collaboration agreement.
* The application must include more than 20 merchants from Apps Bunches’ clients using the app, and this does not represent any direct confirmation of support.

---

## 1. Introduction

Apps Bunches Mobile App is a mobile layer for Zid merchants that allows third-party services to integrate their apps via a **Flutter SDK**.
The SDK communicates with Zid APIs and integrates services seamlessly into the mobile app experience without modifying core functionalities.

---

## 2. Minimum Merchant Requirement

To qualify for integration support inside AppsBunches:

* The partner's solution must be active and used by at least 20 merchants currently using AppsBunches mobile apps.
* This ensures commercial feasibility, operational value, and real adoption.

---

## 3. Mandatory Pre-Approval

Before starting any technical work:

* The partner must contact the AppsBunches Partnerships Team to receive initial approval.
* Initial approval does NOT guarantee final acceptance—technical validation and compliance reviews will follow.

---

## 4. Business Cycle Requirements

### 4.1 App Information

* Full description of the app and its features
* App URL in Zid Apps Market
* Support email and official website (if available)

### 4.2 Media & Demonstration

* Introductory video explaining the app's service
* Clear indication of where the service should appear in the mobile app (e.g., product page, checkout, order success page)

### 4.3 User Stories

* User story describing how the service works on the Zid web store
* User story describing how the service should work on Apps Bunches mobile app

#### Examples

**Chatbot SDK:**
Open the app → Home page → Click on SDK chat icon

**Upselling SDK:**
Open the app → Cart page → Upselling bottom sheet appears automatically → User adds product to cart

**Loyalty Program SDK:**
Open the app → Home page → Points widget should appear
Open the app → Account page → Points widget should appear

---

## 5. Technical Requirements

### 5.1 Flutter SDK Development

* Developers must build a **Flutter SDK**
* SDK must include **full usage documentation**
* SDK must be published on **pub.dev**
* SDK must support the latest **Flutter stable version**

### 5.2 Initialization & Configuration

* SDK must accept initial configuration values in **JSON**
* SDK must support **sandbox mode** for testing
* Sensitive data must be **encrypted**
* Example JSON received from Zid API:

```json
{
  "app_key": "XXXX",
  "status": true,
  "sandbox": false,
  "primary_color": "#FF6600",
  "store_id": "12345",
  "token": "ABCDEF123456",
  "config": {
    "show_button": true,
    "widget_position": "home_page"
  }
}
```

### 5.3 SDK Lifecycle

1. App fetches JSON configuration from Zid API on launch
2. Pass JSON to third-party SDK via init method
3. SDK configures itself based on JSON
4. SDK should handle:

   * Page route tracking
   * Cart updates
   * Store changes

### 5.4 Integration with Zid APIs

* `GET /api/v1/scripts`
* `GET /api/v2/catalog/stores/$storeId/layout-setting`

JSON includes:

* App status
* Tokens & IDs
* Colors
* Any required configuration

### 5.5 Initialization Example

```dart
APPSDKNAME.init(initialJson: {
  'status': true,
  'token': '123456890',
})
.onSuccess((value) => print(value))
.onCatchError((e) => print(e));
```

---

## 6. Optional SDK Features

### 6.1 Page Listener

```dart
APPSDKNAME.trackPageRoute(pageName: "wishlist");
```

### 6.2 Cart Events (CRUD)

```dart
APPSDKNAME.sendEvent(
  cartEvent: CartEvents(
    onAddToCart: (itemCartId) {},
    onDeleteToCart: (itemCartId) {},
  ),
);
```

### 6.3 UI Widgets

```dart
Column(
  children: [
    OrderSuccessHeaderWidget(orderId: widget.orderId),
    OrderDetailsButtonWidget(orderId: widget.orderId),
    APPSDKNAME.orderDetailsWidget(),
    SizedBox(height: 30),
    TransferPaymentWidget(orderId: widget.orderId),
    SizedBox(height: 30),
    VatInfoWidget(),
  ],
);
```

### 6.4 Search Events

```dart
List<Products> products = await APPSDKNAME.getProducts(q: 'Search Item');
```

### 6.5 Abandoned Cart Events

```dart
APPSDKNAME.addCartUpdates(
  deviceId: '123456789',
  customerObject: {},
  cartObject: {},
  phase: 'add_to_cart',
);
```

### 6.6 Customer Support Widgets

```dart
floatingActionButton: APPSDKNAME.showFloatingActionButton();
```

### 6.7 Cashback / Exchange / Return Events

```dart
APPSDKNAME.showWidgets();

APPSDKNAME.return(
  mobileNumber: '+966666',
  userID: '',
  orderModel: {},
);
```

You can also develop any type of widgets and functions needed to showcase your services.

---

## 7. Quality Standards

* SDK must not crash the app
* SDK must not request extra permissions unnecessarily
* SDK must be lightweight and optimized
* SDK must not interfere with Core App functionality
* SDK should provide graceful error handling

---

## 8. Targeted App Categories

* Customer Support Apps
* Marketing Apps
* Operations Apps
* Other Service Apps

---

## 9. Business Approval

Integration testing and a final review meeting must be conducted at the end of the implementation. This ensures that all features are functioning as expected, integration with the AppsBunches mobile app is seamless, and any necessary adjustments can be made before release. The meeting should include a demonstration of key SDK functions, verification of API connectivity, and validation of user experience.

---

### Contact Emails

* **Website:** [www.appsbunches.com](https://www.appsbunches.com)
* **Partnership Email:** [Partnership@appsbunches.com](mailto:Partnership@appsbunches.com)
* **Support & Development Email:** [Dev@appsbunches.com](mailto:Dev@appsbunches.com)
