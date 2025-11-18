# WordPress Shortcode Generator Agent

## Purpose
Automatically generate WordPress shortcodes and embed documentation for calculators, enabling easy integration into WordPress sites.

## Input
- Calculator slug (e.g., `business-loan-dscr`)
- Calculator metadata (name, version, category)
- Embed configuration options (optional customization)

## Process

### Step 1: Read Calculator Metadata
- Read calculator JSON config
- Extract:
  - calculator_slug
  - calculator_name
  - calculator_version
  - category
  - description
- Verify calculator is deployed and accessible

### Step 2: Generate Basic Shortcode

#### 2a. Standard Shortcode Format
```
[smartprofit_calculator slug="business-loan-dscr"]
```

#### 2b. Shortcode with Options
```
[smartprofit_calculator 
  slug="business-loan-dscr" 
  theme="light"
  width="100%"
  height="auto"
  show_title="true"
  show_description="true"
]
```

#### 2c. Define Supported Attributes
- **slug** (required): Calculator identifier
- **theme** (optional): "light" or "dark" (default: "light")
- **width** (optional): CSS width value (default: "100%")
- **height** (optional): CSS height value (default: "auto")
- **show_title** (optional): Display calculator title (default: "true")
- **show_description** (optional): Display description (default: "true")
- **custom_class** (optional): Additional CSS class for styling
- **language** (optional): "en", "es" (future, default: "en")

### Step 3: Generate WordPress Plugin Code

#### 3a. Register Shortcode Handler
Create PHP function for WordPress plugin:
```php
<?php
// File: wordpress-plugins/calculators-plus/shortcodes/business-loan-dscr.php

function smartprofit_calculator_business_loan_dscr($atts) {
    // Parse attributes
    $atts = shortcode_atts(
        array(
            'slug' => 'business-loan-dscr',
            'theme' => 'light',
            'width' => '100%',
            'height' => 'auto',
            'show_title' => 'true',
            'show_description' => 'true',
            'custom_class' => '',
        ),
        $atts,
        'smartprofit_calculator'
    );
    
    // Build iframe URL
    $base_url = 'https://calculators.smartprofit.com';
    $iframe_url = $base_url . '/embed/' . esc_attr($atts['slug']);
    $iframe_url .= '?theme=' . esc_attr($atts['theme']);
    $iframe_url .= '&show_title=' . esc_attr($atts['show_title']);
    $iframe_url .= '&show_description=' . esc_attr($atts['show_description']);
    
    // Build iframe HTML
    $output = '<div class="smartprofit-calculator-wrapper ' . esc_attr($atts['custom_class']) . '">';
    $output .= '<iframe ';
    $output .= 'src="' . esc_url($iframe_url) . '" ';
    $output .= 'width="' . esc_attr($atts['width']) . '" ';
    $output .= 'height="' . esc_attr($atts['height']) . '" ';
    $output .= 'frameborder="0" ';
    $output .= 'style="border: none; min-height: 600px;" ';
    $output .= 'title="' . esc_attr($atts['slug']) . ' Calculator" ';
    $output .= 'loading="lazy"';
    $output .= '></iframe>';
    $output .= '</div>';
    
    // Enqueue responsive script
    wp_enqueue_script('smartprofit-iframe-resizer');
    
    return $output;
}

// Register shortcode
add_shortcode('smartprofit_calculator', 'smartprofit_calculator_business_loan_dscr');
```

#### 3b. Add Iframe Resizing Script
```javascript
// File: wordpress-plugins/calculators-plus/js/iframe-resizer.js

(function() {
    // Listen for resize messages from iframe
    window.addEventListener('message', function(event) {
        if (event.data.type === 'smartprofit-resize') {
            const iframe = document.querySelector('iframe[src*="' + event.data.slug + '"]');
            if (iframe) {
                iframe.style.height = event.data.height + 'px';
            }
        }
    });
})();
```

### Step 4: Generate Embed Documentation

#### 4a. Create Usage Guide
Generate markdown documentation:
```markdown
# Business Loan + DSCR Calculator - WordPress Embed Guide

## Quick Start

Add this shortcode to any WordPress page or post:
```
[smartprofit_calculator slug="business-loan-dscr"]
```

## Customization Options

### Theme
Choose light or dark theme:
```
[smartprofit_calculator slug="business-loan-dscr" theme="dark"]
```

### Size
Control width and height:
```
[smartprofit_calculator slug="business-loan-dscr" width="800px" height="600px"]
```

### Visibility
Hide title or description:
```
[smartprofit_calculator slug="business-loan-dscr" show_title="false" show_description="false"]
```

### Custom Styling
Add custom CSS class:
```
[smartprofit_calculator slug="business-loan-dscr" custom_class="my-custom-style"]