# OTP Angular

`otp-angular` is an Angular OTP input component for verification codes, 2FA/MFA login flows, Web OTP SMS auto-fill, resend countdowns, accessibility, and custom OTP UI. It works with Angular reactive forms, template-driven forms, and direct event binding.

Current version: `1.3.0`

Angular support: Angular 20, 21, and 22

---

![OTP Input Demo](https://github.com/adrik-HubGuru/otp-angular/blob/main/otp-angular.gif?raw=true)

[Demo](https://stackblitz.com/edit/stackblitz-starters-osu6xqrf?file=package.json)
---

## Features

- Standalone Angular component: import `OtpAngular` directly in a standalone component.
- Reactive forms support: works with `formControl`, `formControlName`, and `FormGroup`.
- Template-driven forms support: works with `[(ngModel)]`.
- Manual event support: use `onInputChange`, `onAutoSubmit`, `onResendTimerChange`, and `onResendAvailable`.
- Configurable OTP length: choose 4 digits, 6 digits, or any custom length.
- Numeric-only mode: block non-numeric characters when `numbersOnly` is enabled.
- Alphanumeric mode: allow letters and numbers by default.
- Uppercase mode: convert letters to uppercase with `showCaps`.
- Password mode: hide OTP characters with password-style inputs, with optional banking-style mask delay.
- Auto focus: automatically focus the first input when the component renders in the browser.
- Auto submit: emit the full OTP value when all boxes are filled, with optional valid-only submit control.
- Loading/verifying state: disable OTP boxes and show a compact spinner while verification is running.
- Web OTP SMS auto-fill: optionally fill OTP from supported Android Chrome SMS prompts with `webOtp`.
- Accessibility support: screen-reader labels, live error/resend announcements, and keyboard-only controls.
- Paste support: paste a full OTP and fill the boxes automatically.
- Keyboard navigation: supports Backspace, Delete, Home, End, ArrowLeft, and ArrowRight.
- Disabled state: works through Angular forms disable state and the public `disabled` signal.
- Error state: show Angular form validation messages after touch, with custom messages for cases like invalid or expired OTP.
- Resend countdown: show a resend timer, emit every timer change, and emit when the resend action is clicked.
- Public methods: call `setValue()`, `clear()`, `focus()`, `focusIndex(index)`, and `reset()` from a parent component.
- Custom input template: use `otpAngularInputTemplate` and `otpAngularInput` to fully control each OTP box UI.
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
    autoSubmit: true,
    autoSubmitValidOnly: true,
    webOtp: true
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
| `loading` | `WritableSignal<boolean>` | No | Shows verifying state and blocks user input without disabling the Angular form control. |
| `onInputChange` | `Output<string \| number \| null>` | No | Emits whenever the OTP value changes. |
| `onAutoSubmit` | `Output<string \| number \| null>` | No | Emits when all boxes are filled and `autoSubmit` is true. |
| `onResendTimerChange` | `Output<number>` | No | Emits the remaining resend countdown seconds from the initial value through `0`. |
| `onResendAvailable` | `Output<boolean>` | No | Emits `true` when the resend action is clicked. |
| `setValue(value)` | Method | No | Sets the visible OTP value from the parent component. |
| `clear()` | Method | No | Clears all boxes and updates the Angular form value to `null`. |
| `focus()` | Method | No | Focuses the first OTP box when enabled in the browser; no-op while disabled, loading, or SSR. |
| `focusIndex(index)` | Method | No | Focuses a zero-based OTP box index when enabled in the browser; invalid indexes, disabled/loading state, and SSR are ignored. |
| `reset()` | Method | No | Restarts the resend countdown. |
| `OtpAngularInputTemplate` | Directive | No | Place on an `ng-template` inside `<otp-angular>` to customize each rendered OTP box. |
| `OtpAngularInput` | Directive | No | Place on the real focusable input inside a custom template so focus, paste, arrow keys, and public focus methods keep working. |
| `OtpAngularInputContext` | Type | No | Type for the custom template context passed as `let-otp`. |

## Config Options

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `length` | `number` | `4` | Number of OTP boxes. |
| `numbersOnly` | `boolean` | `false` | Allows only numeric input. |
| `autoSubmit` | `boolean` | `false` | Emits `onAutoSubmit` when the OTP is complete. |
| `autoSubmitValidOnly` | `boolean` | `false` | When `autoSubmit` is true, only emits after accepted input with no rejected or truncated characters and no invalid Angular form state. |
| `autoFocus` | `boolean` | `false` | Focuses the first input in the browser after render. |
| `webOtp` | `boolean` | `false` | Enables Web OTP API SMS auto-fill on supported secure browsers. |
| `isPassword` | `boolean` | `false` | Uses password input type to hide characters. |
| `maskDelay` | `number` | `0` | When `isPassword` is true, briefly shows newly typed or pasted characters for this many milliseconds before masking them again. |
| `showError` | `boolean` | `false` | Adds error styling to empty boxes on blur. |
| `errorMessages` | `Record<string, string>` | `{}` | Custom messages for Angular form errors such as `invalidOtp` or `otpExpired`. |
| `errorMessageClass` | `string` | `''` | CSS class added to the rendered validation message. |
| `loadingLabel` | `string` | `Verifying...` | Text shown beside the loading spinner. |
| `loadingContainerClass` | `string` | `''` | CSS class added to the loading status row. |
| `loadingSpinnerClass` | `string` | `''` | CSS class added to the loading spinner. |
| `ariaLabel` | `string` | `One-time password` | Accessible label for the full OTP input group. |
| `inputAriaLabel` | `string` | `One-time password digit {index} of {length}` | Accessible label template for each OTP input. Supports `{index}` and `{length}`. |
| `resendAriaLabel` | `string` | `Resend verification code` | Accessible label for the resend action button. |
| `resendCountdownAriaLabel` | `string` | `Resend verification code available in {seconds} seconds` | Live countdown announcement template. Supports `{seconds}` and `{label}`. |
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
    <button type="button" (click)="clearOtp()">Clear</button>
    <button type="button" (click)="focusOtp()">Focus</button>
    <button type="button" (click)="focusThirdBox()">Focus third box</button>
    <button type="button" (click)="disableOtp()">Disable</button>
  `
})
export class DemoComponent {
  @ViewChild(OtpAngular) otpRef!: OtpAngular;

  config = signal<OtpAngularType>({ length: 4, resend: 30 });

  fillOtp(): void {
    this.otpRef.setValue('1234');
  }

  clearOtp(): void {
    this.otpRef.clear();
  }

  focusOtp(): void {
    this.otpRef.focus();
  }

  focusThirdBox(): void {
    this.otpRef.focusIndex(2);
  }

  disableOtp(): void {
    this.otpRef.disabled.set(true);
  }

  resetResendTimer(): void {
    this.otpRef.reset();
  }
}
```

## Password Mode With Mask Delay

Use `isPassword` to mask OTP boxes. Add `maskDelay` when you want newly typed or pasted characters to show briefly before they are hidden again.

```ts
import { Component, signal } from '@angular/core';
import { OtpAngular, OtpAngularType } from 'otp-angular';

@Component({
  imports: [OtpAngular],
  template: `<otp-angular [config]="config()" />`
})
export class PasswordOtpComponent {
  config = signal<OtpAngularType>({
    length: 6,
    numbersOnly: true,
    isPassword: true,
    maskDelay: 500
  });
}
```

`maskDelay` works only when `isPassword` is true. Programmatic values from forms, `setValue()`, and Web OTP stay masked immediately.

## Custom Input Template

Use `otpAngularInputTemplate` when the default input markup is not enough. The template receives one context object per box as `let-otp`. Put `otpAngularInput` on the real input element so the component can still manage focus, paste, Backspace, arrow keys, `focus()`, and `focusIndex(index)`.

```ts
import { Component, signal } from '@angular/core';
import { OtpAngular, OtpAngularInput, OtpAngularInputTemplate, OtpAngularType } from 'otp-angular';

@Component({
  imports: [OtpAngular, OtpAngularInput, OtpAngularInputTemplate],
  template: `
    <otp-angular [config]="config()">
      <ng-template otpAngularInputTemplate let-otp>
        <input
          otpAngularInput
          maxlength="1"
          autocomplete="one-time-code"
          [value]="otp.value"
          [disabled]="otp.disabled"
          [readOnly]="otp.readonly"
          [type]="otp.type"
          [placeholder]="otp.placeholder"
          [class]="otp.inputClass"
          [style]="otp.inputStyle"
          [class.pin-box]="true"
          [attr.aria-label]="otp.ariaLabel"
          [attr.aria-invalid]="otp.ariaInvalid"
          [attr.aria-describedby]="otp.ariaDescribedBy"
          [attr.inputmode]="otp.inputMode"
          [attr.pattern]="otp.pattern"
          [class.error]="otp.hasError"
          (keydown)="otp.handlers.keydown($event)"
          (blur)="otp.handlers.blur($event)"
          (input)="otp.handlers.input($event)"
          (paste)="otp.handlers.paste($event)"
        />
      </ng-template>
    </otp-angular>
  `
})
export class CustomOtpComponent {
  config = signal<OtpAngularType>({
    length: 6,
    numbersOnly: true,
    autoSubmit: true
  });
}
```

The context also includes `index`, `loading`, `componentDisabled`, `blocked`, `inputClass`, `inputStyle`, `separator`, `isLast`, `maxlength`, `autocomplete`, and `name` for richer custom UIs.

## Custom Resend Timer UI

Use `onResendTimerChange` when your app wants to show the countdown outside the built-in resend row. The built-in resend UI still works as before.

```ts
import { Component, signal } from '@angular/core';
import { OtpAngular, OtpAngularType } from 'otp-angular';

@Component({
  imports: [OtpAngular],
  template: `
    <otp-angular
      [config]="config()"
      (onResendTimerChange)="remainingSeconds.set($event)"
      (onResendAvailable)="resendCode()"
    />

    <p>Try again in {{ remainingSeconds() }} seconds</p>
  `
})
export class CustomResendComponent {
  config = signal<OtpAngularType>({ length: 6, resend: 30 });
  remainingSeconds = signal(30);

  resendCode(): void {
    console.log('Resend clicked');
  }
}
```

## Web OTP SMS Auto-Fill

Enable browser-assisted SMS OTP filling with `webOtp: true`:

```ts
config: OtpAngularType = {
  length: 6,
  numbersOnly: true,
  autoSubmit: true,
  webOtp: true
};
```

Web OTP works mainly on supported Android Chrome browsers and requires a secure context such as HTTPS. Your backend must send a Web OTP formatted SMS that includes the site domain and OTP code. Unsupported browsers, denied permission, SSR, insecure pages, and iframes without `Permissions-Policy: otp-credentials` support fall back to normal manual typing and paste behavior.

## Strict Auto Submit

Use `autoSubmitValidOnly` when your app should auto-submit only after clean, accepted input. Rejected paste characters still update the visible OTP from the accepted part, but `onAutoSubmit` will not fire for that paste.

```ts
config: OtpAngularType = {
  length: 6,
  numbersOnly: true,
  autoSubmit: true,
  autoSubmitValidOnly: true
};
```

For example, pasting `12a345` into a numeric OTP can fill the accepted digits, but the component will wait for a clean completion before emitting `onAutoSubmit`.

## Accessibility Labels

The component includes a labelled OTP group, per-box labels, assertive form-error announcements, and polite resend countdown announcements. You can customize the screen-reader text without changing the visible UI.

```ts
config: OtpAngularType = {
  length: 6,
  numbersOnly: true,
  ariaLabel: 'Login verification code',
  inputAriaLabel: 'Verification code digit {index} of {length}',
  resendAriaLabel: 'Send a new verification code',
  resendCountdownAriaLabel: 'New code available in {seconds} seconds'
};
```

Keyboard users can move with ArrowLeft and ArrowRight, jump with Home and End, remove the current box with Delete, and use Backspace as usual.

## Loading / Verifying State

Use the public `loading` signal when your app starts server-side verification. This keeps the visible OTP value in place, blocks user typing and paste, and shows a small status row until you turn loading off.

```ts
import { Component, ViewChild, signal } from '@angular/core';
import { FormControl, ReactiveFormsModule } from '@angular/forms';
import { OtpAngular, OtpAngularType } from 'otp-angular';

@Component({
  imports: [OtpAngular, ReactiveFormsModule],
  template: `
    <otp-angular
      [config]="config()"
      [formControl]="otp"
      (onAutoSubmit)="verifyOtp($event)"
    />
  `
})
export class VerifyOtpComponent {
  @ViewChild(OtpAngular) otpRef!: OtpAngular;

  config = signal<OtpAngularType>({
    length: 6,
    numbersOnly: true,
    autoSubmit: true,
    loadingLabel: 'Checking code...'
  });

  otp = new FormControl<string | number | null>('');

  async verifyOtp(value: string | number | null): Promise<void> {
    this.otpRef.loading.set(true);
    try {
      await this.verifyCodeOnServer(value);
    } finally {
      this.otpRef.loading.set(false);
    }
  }

  private verifyCodeOnServer(value: string | number | null): Promise<void> {
    return Promise.resolve();
  }
}
```

## Form Error Messages

`otp-angular` can read Angular form errors from reactive forms and `ngModel`. It shows one message after the control is touched and invalid, and adds error styling plus `aria-invalid` to the OTP boxes.

```ts
import { Component, signal } from '@angular/core';
import { FormControl, ReactiveFormsModule, Validators } from '@angular/forms';
import { OtpAngular, OtpAngularType } from 'otp-angular';

@Component({
  imports: [OtpAngular, ReactiveFormsModule],
  template: `<otp-angular [config]="config()" [formControl]="otp" />`
})
export class VerifyOtpComponent {
  config = signal<OtpAngularType>({
    length: 6,
    numbersOnly: true,
    errorMessages: {
      invalidOtp: 'Invalid OTP',
      otpExpired: 'OTP expired'
    }
  });

  otp = new FormControl<string | number | null>('', {
    validators: [Validators.required, Validators.minLength(6)]
  });

  rejectCode(): void {
    this.otp.markAsTouched();
    this.otp.setErrors({ invalidOtp: true });
  }

  expireCode(): void {
    this.otp.markAsTouched();
    this.otp.setErrors({ otpExpired: true });
  }
}
```

Built-in messages are available for `required`, `minlength`, `maxlength`, `pattern`, `invalidOtp`, and `otpExpired`. If a key is not known and no custom message is configured, the component falls back to `Invalid OTP`.

## Version 1.3.0 Notes

- Adds optional Web OTP SMS auto-fill with `webOtp: true`.
- Uses the existing form/value pipeline, so Web OTP updates visible boxes, reactive forms, `ngModel`, `onInputChange`, and `onAutoSubmit`.
- Falls back silently when Web OTP is unsupported, unavailable, denied, aborted, or rendered on the server.
- Adds Angular-form-aware error messages for touched invalid controls.
- Supports custom `errorMessages` and `errorMessageClass` config while keeping the existing `showError` blur styling.
- Adds a parent-controlled loading/verifying state with `otpRef.loading.set(true)`.
- Adds public `clear()`, `focus()`, and `focusIndex(index)` methods for parent-controlled OTP flows.
- Adds `onResendTimerChange` so parent apps can render custom resend timer UI from the same countdown.
- Adds `otpAngularInputTemplate` and `otpAngularInput` for advanced custom OTP box markup without forking the library.
- Adds `maskDelay` for password-mode OTP boxes, so newly typed or pasted characters can briefly show before masking.
- Adds `autoSubmitValidOnly` so apps can block automatic submit after rejected or truncated paste input.
- Improves accessibility with configurable screen-reader labels, assertive error announcements, polite resend countdown announcements, native resend button markup, and Home/End/Delete keyboard support.

## Version 1.2.1 Notes

- Refreshes the npm README so users can see the full feature list, examples, config options, and release workflow directly on the package page.
- Keeps the same public API and Angular 20-22 compatibility from `1.2.0`.

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
  "version": "1.3.0"
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
cd dist/otp-angular
npm publish --access public
```

## 📄 License

[![License: MIT](https://raw.githubusercontent.com/subha-patra/otp-angular/ce74d1caa98e055864f1dab0b4dd7be6477589e4/licence.svg)](LICENSE)
![npm](https://img.shields.io/npm/v/otp-angular)
![npm](https://img.shields.io/npm/dt/otp-angular)
![GitHub issues](https://img.shields.io/github/issues/subha-patra/otp-angular)
![GitHub stars](https://img.shields.io/github/stars/subha-patra/otp-angular)
![GitHub license](https://img.shields.io/github/license/subha-patra/otp-angular)
