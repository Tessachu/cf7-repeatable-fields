# Contact Form 7 - Repeatable Fields

> Adds repeatable groups of fields to Contact Form 7.

[![Support Level](https://img.shields.io/badge/support-may_take_time-yellow.svg)](#support-level) [![Tests Status](https://github.com/felipeelia/cf7-repeatable-fields/actions/workflows/test.yml/badge.svg?branch=develop)](https://github.com/felipeelia/cf7-repeatable-fields) [![Release Version](https://img.shields.io/github/release/felipeelia/cf7-repeatable-fields.svg)](https://github.com/felipeelia/cf7-repeatable-fields/releases/latest) ![WordPress tested up to version](https://img.shields.io/wordpress/plugin/tested/cf7-repeatable-fields?label=WordPress) [![GPLv2 License](https://img.shields.io/github/license/felipeelia/cf7-repeatable-fields.svg)](https://github.com/felipeelia/cf7-repeatable-fields/blob/trunk/LICENSE.md)

## Requirements

This plugin requires these software with the following versions:

* [WordPress](https://wordpress.org) 6.0+
* [PHP](https://php.net/) 7.2+
* [Contact Form 7](https://wordpress.org/plugins/contact-form-7/) 5.7+

## Usage ##

### Form tab ###
Wrap the desired fields with `[field_group your_group_id_here][/field_group]`. The shortcode accepts additional parameters, in WP shortcode format and in CF7 fields parameters format as well.

Example:
```html
[field_group emails id="emails-groups" tabindex:1]
	<label>Your Email (required)[email* your-email]</label>
	[radio your-radio use_label_element default:1 "radio 1" "radio 2" "radio 3"]
	[select* your-menu include_blank "option1" "option 2"]
	[checkbox* your-checkbox "check 1" "check 2"]
[/field_group]
```

### Mail tab ###
In the mail settings, wrap the fields with your group id. You can use the `[group_index]` tag to print the group index and an additional `__<NUMBER>` to print a field at a specific index.

Example:
```html
The second email entered by the user was: [your-email__2]

These were the groups:
[emails]
GROUP #[group_index]
	Checkbox: [your-checkbox]
	E-mail: [your-email]
	Radio: [your-radio]
	Select: [your-menu]
[/emails]
```

## Customizing the add and remove buttons ##
You can [add filters](https://developer.wordpress.org/reference/functions/add_filter/) to your theme to customize the add and remove buttons.

Example
```php
// In your theme's functions.php
function customize_add_button_atts( $attributes ) {
  return array_merge( $attributes, array(
    'text' => 'Add Entry',
  ) );
}
add_filter( 'wpcf7_field_group_add_button_atts', 'customize_add_button_atts' );
```

The available filters are:

### wpcf7_field_group_add_button_atts ###

Filters the add button attributes.

Parameters:
 * `$attributes`: Array of attributes for the add button. Keys:
	 * `group_id`: group ID
	 * `additional_classes`: css class(es) to add to the button
	 * `text`: text used for the button

Return value: array of button attributes

### wpcf7_field_group_add_button ###

Filters the add button HTML.

Parameters:
* `$html`: Default add button HTML
* `$group_id`: The group ID

Return value: button HTML

### wpcf7_field_group_remove_button_atts ###

Filters the remove button attributes.

Parameters:
 * `$attributes`: Array of attributes for the remove button. Keys:
	 * `group_id`: group ID
	 * `additional_classes`: css class(es) to add to the button
	 * `text`: text used for the button

Return value: array of button attributes

### wpcf7_field_group_remove_button ###

Filters the remove button HTML.

Parameters:
* `$html`: Default remove button HTML
* `$group_id`: The group ID

Return value: button HTML

## Contribute ##
You can contribute with code, issues and ideas at the [GitHub repository](https://github.com/felipeelia/cf7-repeatable-fields).

If you like it, a review is appreciated :)

## Frequently Asked Questions ##

### Can I change the add/remove buttons? ###

Yes. You can use `wpcf7_field_group_add_button_atts`, `wpcf7_field_group_add_button`, `wpcf7_field_group_remove_button_atts`, and `wpcf7_field_group_remove_button` filters, as shown above. Props to @berniegp.

### How can I display the group index number in the form? ###

You'll have to use the `wpcf7-field-groups/change` jQuery event.

In the Form tab, add an element to hold the group index. In this example, it'll be a `<span>` with the `group-index` class:
```html
[field_group emails id="emails-groups" tabindex:1]
	<p>Group #<span class="group-index"></span></p>
	<label>Your Email (required)[email* your-email]</label>
	[radio your-radio use_label_element default:1 "radio 1" "radio 2" "radio 3"]
	[select* your-menu include_blank "option1" "option 2"]
	[checkbox* your-checkbox "check 1" "check 2"]
[/field_group]
```

And then you’ll have to add this to your JavaScript code:
```js
jQuery(function($) {
	$('.wpcf7-field-groups').on('wpcf7-field-groups/change', function() {
		var $groups = $(this).find('.group-index');
		$groups.each(function() {
			$(this).text($groups.index(this) + 1);
		} );
	}).trigger('wpcf7-field-groups/change');
});
```

You can add that JS through your theme OR use some plugin like [Simple Custom CSS and JS](https://wordpress.org/plugins/custom-css-js/).

## Changelog

A complete listing of all notable changes to this plugin are documented in [CHANGELOG.md](https://github.com/felipeelia/cf7-repeatable-fields/blob/trunk/CHANGELOG.md).
