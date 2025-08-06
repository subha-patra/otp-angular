# 🔐 OTP Angular

**Otp Angular** is a lightweight, highly customizable, and dependency-free OTP (One-Time Password) input component built for Angular 20+ applications. It offers seamless integration, flexible configuration, and a polished user experience for OTP or verification inputs. The component now also includes a built-in **resend option**, making it easy to handle OTP resubmission flows directly within the UI.

> ✅ Supports Angular 20+  
> 🔧 Fully customizable  
> 🎯 Keyboard navigation support  
> 🧪 Easily testable & maintainable  
> 💡 Auto-focus, password-style, number-only, and more

---

![OTP Input Demo](https://github.com/adrik-HubGuru/otp-angular/blob/main/otp-angular.gif?raw=true)

[Demo](https://stackblitz.com/edit/stackblitz-starters-osu6xqrf?file=package.json)
---
## 📦 Installation

```bash
npm install otp-angular
```
--- 

## 🚀 Latest changes in 1.0.4

- **Update Readme** Readme file updated
 

---

## 🚀 Latest changes in 1.0.3

- **Support ReactiveFormsModule & FormsModule** formControl and ngModel are supported
- **Support types on Config** types will be available
- **Fixed Paste Event** types will be available

---

## 🚀 latest Changes in 1.0.1

- **Emits `null` instead of an empty string** if no value is supplied
- **Resend Option added** if you add resend as option then it will open

---


## Usage


For Component

```bash
import { OtpAngular, OtpAngularType } from 'otp-angular';

@Component({
  imports: [...others, OtpAngular, OtpAngularType]
})

export class <ComponentName> {
  protected config = signal<OtpAngularType>({ length: 4 });
}

```

For Template
```bash
@let _config = config();
<!-- With Event Binding -->
<otp-angular [config]="_config" (onInputChange)="onInputChange($event)"  (onResendAvailable)="resend($event)" />

<!-- Using Reactive FormControl -->
<otp-angular [config]="_config" formControlName="otp" />

<!-- Using Forms Module -->
<otp-angular [config]="_config" [(ngModel)]="otp" />

```

--- 


## ⚙️ Inputs/Outputs

 | Option            | Type                      |required    | Description                    | Default|
|-------------------|---------------------------|-------------|-------------------------------|---------|
| `disabled`        | `boolean`                 |    No       | Disables otp when set to true | `false` |
| `onInputChange`   | `Output`                  |    No       | Emits the OTP value on change, it's return `string`, `number` or `null` | —       |
| `onResendAvailable`| `Output`                 |    No       | Emits when you click resend option, as a boolean(true)  | —       |
| `setValue`        | `function`                |    No       | Set the otp value             | —       |
| `reset`           | `function`                |    No       | Reset the Resend              | —       |
| `config`          | `object`                  |    Yes      | Configure based on option. (see Config Options below)   | `{ length: 4 }` |



## ⚙️ Config Options


 | Option            | Type                      |required    | Description                    | Default|
|-------------------|---------------------------|-------------|-------------------------------|---------|
| `length`          | `number`                  |    Yes      | Number of OTP digits          | 4       |
| `numbersOnly`     | `boolean`                 |    No       | Allow only numeric input      | `false` |
| `autoFocus`       | `boolean`                 |    No       | Auto-focus first input        | `false` |
| `isPassword`      | `boolean`                 |    No       | Mask input characters         | `false` |
| `showError`       | `boolean`                 |    No       | Show red border on error      | `false` |
| `showCaps`        | `boolean`                 |    No       | Transform to Capital letters  | `false` |
| `containerClass`  | `string` or `string[]`    |    No       | Custom CSS class for container| —       |
| `containerStyles` | `object`                  |    No       | Inline styles for container   | `{}`    |
| `inputClass`      | `string` or `string[]`    |    No       | Custom class for input boxes  | —       |
| `inputStyles`     | `object` or `object[]`    |    No       | Inline styles for input fields| `{}`    |
| `placeholder`     | `string`                  |    No       | Placeholder for each input box| `''`    |
| `separator`       | `string`                  |    No       | Optional separator character  | `''`    |
| `resend`          | `number`                  |    No       | Enable the Resend option      | `0`    |
| `resendLabel`     | `string`                  |    No       | Label for Resend              | `RESEND VERIFICATION CODE`    |
| `resendContainerClass`   | `string`           |    No       | Custom class for resend container  | `''`    |
| `resendLabelClass`       | `string`           |    No       | Optional class for resend Label    | `''`    |
| `resendTimerClass`       | `string`           |    No       | Optional class for resend timer    | `''`    |



---



### 📘 Option Descriptions

- **`length`**: Sets how many input boxes are shown (e.g., 6 for a 6-digit OTP).
- **`numbersOnly`**: If true, only numeric input is allowed in each box.
- **`autoFocus`**: Automatically focuses the first input box on load.
- **`isPassword`**: Hides characters behind dots, like a password field.
- **`showError`**: Enables error styling (e.g., red border).
- **`showCaps`**: Transform to Capital letters .
- **`containerClass` / `inputClass` / `resendContainerClass` / `resendLabelClass` / `resendTimerClass` **: Lets you add your own CSS classes for styling.
- **`containerStyles` / `inputStyles`**: Set inline styles directly from your component.
- **`placeholder`**: The character shown in empty input boxes (like `*` or `_`).
- **`separator`**: Visual separator between input boxes (like `-` or `:`).
- **`resend`**: Sets the value to show the Resend option, value will be in seconds (like `60`).
- **`resendLabel`**: To change the label for resend (e.g., `Resend Code`).

---

## 🧩 Other Features
Use @ViewChild to get a reference to the component
```bash
@ViewChild(OtpAngular, { static: false }) otpRef!: OtpAngular;
```


### 🔒 Disabling Inputs

You can disable all input fields using the `disabled` input or programmatically:

```bash
this.otpRef.disabled.set(true);
```


### 🔁 Updating OTP Value

```bash
this.otpRef.setValue('1234');
```

### 🔁 Reset the timer of resend

```bash
this.otpRef.reset();
```

---

## 📄 License

[![License: MIT](https://raw.githubusercontent.com/subha-patra/otp-angular/ce74d1caa98e055864f1dab0b4dd7be6477589e4/licence.svg)](LICENSE)
![npm](https://img.shields.io/npm/v/otp-angular)
![npm](https://img.shields.io/npm/dt/otp-angular)
![GitHub issues](https://img.shields.io/github/issues/subha-patra/otp-angular)
![GitHub stars](https://img.shields.io/github/stars/subha-patra/otp-angular)
![GitHub license](https://img.shields.io/github/license/subha-patra/otp-angular)
