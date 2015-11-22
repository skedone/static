---
layout: post
title:  "Woocommerce breadcrumbs: schema.org / RDFa / microdata"
date:   2015-11-22 03:02:00
categories: 
---

Woocommerce doesn't provide good methods to deal with breadcrumbs, this is a fact. Implementing a schema.org or RDFa or microdata needs to override the function responsible for generating the breadcrumb itself. **Yes, life sucks**, especially when you use Wordpress as an ecommerce system and you have to use Woocommerce.

Fortunately, you can override Woocommerce's function copy-pasting the file ```woocommerce/global/breadcrumbs.php``` from the Woocommerce's plugin directory to you theme directory.

```bash
$ mkdir -p wp-content/theme/[your_theme]/woocommerce/global/
$ cp wp-content/plugins/woocommerce/global/breadcrumbs.php /wp-content/theme/[your_theme]/woocommerce/global/breadcrumbs.php
```

At the lines [23-27] you can find the piece of the function you need to modify. 

```php
<?php
// woocommerce/global/breadcrumbs.php
if ( ! empty( $crumb[1] ) && sizeof( $breadcrumb ) !== $key + 1 ) {
	echo '<a href="' . esc_url( $crumb[1] ) . '">' . esc_html( $crumb[0] ) . '</a>';
} else {
	echo esc_html( $crumb[0] );
}

```

This is not really fancy nor clean. This is just a workaround.

```php
<?php
// woocommerce/global/breadcrumbs.php
if ( ! empty( $crumb[1] ) && sizeof( $breadcrumb ) !== $key + 1 ) {
	echo '<span property="itemListElement" typeof="ListItem"><a href="' . esc_url( $crumb[1] ) . '" property="item" typeof="WebPage"><span property="name">' . esc_html( $crumb[0] ) . '</span></a><meta property="position" content="' . ($key + 1) . '"></span>';
} else {
	echo esc_html( $crumb[0] );
}
```

You can now hook inside Woocommerce ```woocommerce_breadcrumb_defaults``` function and display your new breadcrumbs. 

```php
<?php 
// [your_theme]/function.php

add_filter('woocommerce_breadcrumb_defaults', 'schema_breadcrumb');

function schema_breadcrumb($defaults) {
    $defaults = array(
            'delimiter'   => ' &gt; ',
            'wrap_before' => '<nav class="woocommerce-breadcrumb" vocab="http://schema.org/" typeof="BreadcrumbList">',
            'wrap_after'  => '</nav>',
            'before'      => '',
            'after'       => '',
            'home'        => _x( 'Home', 'breadcrumb', 'woocommerce' ),
        );
    return $defaults;
}
```

I know, life sucks.