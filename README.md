# Ember-power-select-with-fallback

This is a addon on top Ember Power Select that based on some criteria decides if renders the rich `{{#power-select}}`
component or fallbacks to a simpler `<select>`.

This is particularly useful if you want to use native selects on iOS/Android devices, when
screen readers are detected, or any other criteria you decide.

Take into account that since this components will try to fallback to a simple select, many features
availables in Ember Power Select won't work with the fallback.

The component during development will try to warn you when you use some feature that cannot be safely
translated to a regular select. **TODO: Not done :trollface: **

**Disclaimer:** This component is the fruit of 1h of work. I'm sure it can be greatly improved. PR welcomed.

## DEMO

[https://ember-power-select-with-fallback.pagefrontapp.com/](https://ember-power-select-with-fallback.pagefrontapp.com/)

## Installation

* `ember install ember-power-select-with-fallback`

## Usage

If your options are just strings, this component is a drop in replacement of ember-power-select.

```hbs
{{#power-select-with-fallback options=options selected=selected onchange=(action (mut selected)) as |opt|}}
  {{opt}}
{{/power-select-with-fallback}}
```

If your options are objects, you will need to pass a `labelPath` property to hint the component what
field should display when it fallback to the native component.


```hbs
{{#power-select-with-fallback options=users selected=user onchange=(action (mut user)) labelPath="fullName" as |opt|}}
  {{opt.fullName}}
{{/power-select-with-fallback}}
```

If you by default you will see that non of the previous examples is displayed as a native select. Why?
Because fallback to native is an opt-in behaviour.

You can either pass `mustFallback=<true|false>` to customize this behaviour:

```hbs
{{#power-select-with-fallback mustFallback=true options=options selected=selected onchange=(action (mut selected)) as |opt|}}
  {{opt}}
{{/power-select-with-fallback}}
```

or, more likely, specify a fallback strategy, with `fallback-when=<fallback-strategy>`.
There is 4 fallback strategies included in the component:

* `ios`: Fallbacks only in ios devices.
* `android`: Fallbacks only in android devices.
* `windows-phone`: Fallbacks only in windows phones.
* `mobile`: Fallbacks in any of the previous cases.

```hbs
{{#power-select-with-fallback fallback-when="mobile" options=options selected=selected onchange=(action (mut selected)) as |opt|}}
  {{opt}}
{{/power-select-with-fallback}}

## Known limitations

* All the options must be available upfront. You cannot populate options using the `search` action.
* Accepts nested groups up to 1 level deep.

## Upcoming features

* Implement some developer warnings that make very clear to the users that they're using some functionality
that cannot be translated.

* Support passing a promise that resolves to a collection as `options`. The component should be disabled
until that promise resolves.

* Fallback multiple select too.
