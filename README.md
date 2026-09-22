# OTP Angular

`otp-angular` is a lightweight OTP input component for Angular applications. It works with Angular reactive forms, template-driven forms, and direct event binding.

Current version: `1.2.0`

Angular support: Angular 20, 21, and 22

---

![OTP Input Demo](https://github.com/adrik-HubGuru/otp-angular/blob/main/otp-angular.gif?raw=true)

[Demo](https://stackblitz.com/edit/stackblitz-starters-osu6xqrf?file=package.json)
---

## Features

- Standalone Angular component: import `OtpAngular` directly in a standalone component.
- Reactive forms support: works with `formControl`, `formControlName`, and `FormGroup`.
- Template-driven forms support: works with `[(ngModel)]`.
- Manual event support: use `onInputChange`, `onAutoSubmit`, and `onResendAvailable`.
- Configurable OTP length: choose 4 digits, 6 digits, or any custom length.
- Numeric-only mode: block non-numeric characters when `numbersOnly` is enabled.
- Alphanumeric mode: allow letters and numbers by default.
- Uppercase mode: convert letters to uppercase with `showCaps`.
- Password mode: hide OTP characters with password-style inputs.
- Auto focus: automatically focus the first input when the component renders in the browser.
- Auto submit: emit the full OTP value when all boxes are filled.
- Paste support: paste a full OTP and fill the boxes automatically.
- Keyboard navigation: supports Backspace, ArrowLeft, and ArrowRight.
- Disabled state: works through Angular forms disable state and the public `disabled` signal.
- Error state: optionally marks empty boxes on blur with `showError`.
- Resend countdown: show a resend timer and emit when the resend action is clicked.
- Public methods: call `setValue()` and `reset()` from a parent component.
- Custom styling: pass custom classes and inline styles for the container and input boxes.
- Per-input styling: pass arrays for input classes or styles.
- Separator support: show a separator between OTP inputs.
- Theme support: built-in `light` and `dark` theme option.
- Multiple-instance safe: multiple OTP components can be used on the same page.
- SSR-safe: avoids browser-only focus, timer, and clipboard behavior on the server.
- No runtime dependency other than Angular and `tslib`.

## Installation

```bash
npm install otp-angular
```

## Quick Start

### Standalone Component

```ts
import { Component, signal } from '@angular/core';
import { OtpAngular, OtpAngularType } from 'otp-angular';

@Component({
  selector: 'app-login',
  imports: [OtpAngular],
  template: `
    <otp-angular
      [config]="config()"
      (onInputChange)="onInputChange($event)"
      (onAutoSubmit)="verifyOtp($event)"
    />
  `
})
export class LoginComponent {
  config = signal<OtpAngularType>({
    length: 6,
    numbersOnly: true,
    autoFocus: true,
    autoSubmit: true
  });

  onInputChange(value: string | number | null): void {
    console.log(value);
  }

  verifyOtp(value: string | number | null): void {
    console.log(value);
  }
}
```

### Reactive Forms

```ts
import { Component, signal } from '@angular/core';
import { FormControl, ReactiveFormsModule } from '@angular/forms';
import { OtpAngular, OtpAngularType } from 'otp-angular';

@Component({
  selector: 'app-otp-form',
  imports: [OtpAngular, ReactiveFormsModule],
  template: `
    <otp-angular [config]="config()" [formControl]="otp" />
  `
})
export class OtpFormComponent {
  config = signal<OtpAngularType>({ length: 4, numbersOnly: true });
  otp = new FormControl<string | number | null>('');

  fillDemoValue(): void {
    this.otp.setValue('1234');
  }
}
```

### Template-Driven Forms

```ts
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { OtpAngular, OtpAngularType } from 'otp-angular';

@Component({
  selector: 'app-ng-model-otp',
  imports: [OtpAngular, FormsModule],
  template: `
    <otp-angular [config]="config" [(ngModel)]="otp" />
  `
})
export class NgModelOtpComponent {
  config: OtpAngularType = { length: 4 };
  otp: string | number | null = '';
}
```

## Inputs, Outputs, And Methods

| API | Type | Required | Description |
| --- | --- | --- | --- |
| `config` | `OtpAngularType` | Yes | Main configuration object. |
| `disabled` | `WritableSignal<boolean>` | No | Programmatically disables or enables all boxes. |
| `onInputChange` | `Output<string \| number \| null>` | No | Emits whenever the OTP value changes. |
| `onAutoSubmit` | `Output<string \| number \| null>` | No | Emits when all boxes are filled and `autoSubmit` is true. |
| `onResendAvailable` | `Output<boolean>` | No | Emits `true` when the resend action is clicked. |
| `setValue(value)` | Method | No | Sets the visible OTP value from the parent component. |
| `reset()` | Method | No | Restarts the resend countdown. |

## Config Options

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `length` | `number` | `4` | Number of OTP boxes. |
| `numbersOnly` | `boolean` | `false` | Allows only numeric input. |
| `autoSubmit` | `boolean` | `false` | Emits `onAutoSubmit` when the OTP is complete. |
| `autoFocus` | `boolean` | `false` | Focuses the first input in the browser after render. |
| `isPassword` | `boolean` | `false` | Uses password input type to hide characters. |
| `showError` | `boolean` | `false` | Adds error styling to empty boxes on blur. |
| `showCaps` | `boolean` | `false` | Converts letters to uppercase. |
| `containerClass` | `string \| string[]` | `''` | CSS class or classes for the OTP container. |
| `containerStyles` | `object` | `{}` | Inline styles for the OTP container. |
| `inputClass` | `string \| string[]` | `''` | CSS class or classes for input boxes. |
| `inputStyles` | `object \| object[]` | `{}` | Inline styles for input boxes. |
| `placeholder` | `string` | `''` | Placeholder shown inside each input. |
| `separator` | `string` | `''` | Character shown between input boxes. |
| `resend` | `number` | `0` | Enables resend countdown when greater than `0`. |
| `resendLabel` | `string` | `RESEND VERIFICATION CODE` | Text shown for resend action/timer. |
| `resendContainerClass` | `string` | `''` | CSS class for the resend wrapper. |
| `resendLabelClass` | `string` | `''` | CSS class for the resend action label. |
| `resendTimerClass` | `string` | `''` | CSS class for the countdown text. |
| `theme` | `'light' \| 'dark'` | `'light'` | Built-in theme mode. |

## Public Method Example

```ts
import { Component, ViewChild, signal } from '@angular/core';
import { OtpAngular, OtpAngularType } from 'otp-angular';

@Component({
  imports: [OtpAngular],
  template: `
    <otp-angular [config]="config()" />
    <button type="button" (click)="fillOtp()">Fill</button>
    <button type="button" (click)="disableOtp()">Disable</button>
  `
})
export class DemoComponent {
  @ViewChild(OtpAngular) otpRef!: OtpAngular;

  config = signal<OtpAngularType>({ length: 4, resend: 30 });

  fillOtp(): void {
    this.otpRef.setValue('1234');
  }

  disableOtp(): void {
    this.otpRef.disabled.set(true);
  }

  resetResendTimer(): void {
    this.otpRef.reset();
  }
}
```

## Version 1.2.0 Notes

- Builds and tests with Angular 22.
- Keeps Angular 20, 21, and 22 peer dependency support.
- Uses signal state for the OTP value.
- Uses Angular template refs instead of global DOM IDs.
- Removes internal helper directives for input filtering and disabled state.
- Fixes external form writes so `formControl.setValue()`, `[(ngModel)]`, and `setValue()` update the visible boxes.
- Emits `null` for an empty numeric OTP instead of `0`.
- Supports multiple OTP components on the same page without focus/value conflicts.
- Adds explicit SSR/browser guards for focus, timers, and legacy clipboard fallback.
- Expands tests for forms, paste, auto-submit, disabled state, multiple instances, keyboard behavior, and server-platform creation.

## Updating The Version

Update the version in:

```text
projects/otp-angular/package.json
```

Example:

```json
{
  "name": "otp-angular",
  "version": "1.2.1"
}
```

Keep peer dependencies compatible with Angular 20-22 unless support changes:

```json
{
  "@angular/common": "^20.0.0 || ^21.0.0 || ^22.0.0",
  "@angular/core": "^20.0.0 || ^21.0.0 || ^22.0.0",
  "@angular/forms": "^20.0.0 || ^21.0.0 || ^22.0.0"
}
```

Before publishing:

```bash
npm run ng -- build otp-angular
npm run ng -- test otp-angular --watch=false --browsers=ChromeHeadless --progress=false
npm run build
git diff --check
```

Check the built package version:

```bash
node -p "require('./dist/otp-angular/package.json').version"
```

Publish from the built package:

```bash
npm publish dist/otp-angular --access public
```

## 📄 License

[![License: MIT](https://raw.githubusercontent.com/subha-patra/otp-angular/ce74d1caa98e055864f1dab0b4dd7be6477589e4/licence.svg)](LICENSE)
![npm](https://img.shields.io/npm/v/otp-angular)
![npm](https://img.shields.io/npm/dt/otp-angular)
![GitHub issues](https://img.shields.io/github/issues/subha-patra/otp-angular)
![GitHub stars](https://img.shields.io/github/stars/subha-patra/otp-angular)
![GitHub license](https://img.shields.io/github/license/subha-patra/otp-angular)
