---
title: Add Catalog Customizations
excerpt: This guide shows you how to implement user-defined customizations for the products in your catalog.
---

# Add Catalog Customizations

Customizations are pre-defined fields in your product that users can edit and create custom versions of your projects, such as for custom engravings.

## Step 1: Create a `Catalog::Customizations` subclass

The first step is to subclass `Workarea::Catalog::Customizations` with something descriptive of your customization. In this case, we'll be using the concept of an "engraving" for jewelry or other metal products:

```ruby
module Workarea
  module Catalog
    class Customizations::Engraving < Customizations
      customized_fields :initials
      validates_presence_of :initials
    end
  end
end
```

This validates the `:initials` field when products that use this customization are ordered.

## Step 2: Apply Customizations to the Product

Choose the product that you wish to apply customizations on, and set the `:customizations` field on the model:

```ruby
product = Workarea::Catalog::Product.find('0-XABC12345')
product.update! customizations: 'engraving'
```

Now, any time someone orders this product, they will have the option to engrave it.

## Step 3: (optional) Apply a Pricing SKU to the Customization.

To charge for customizations, you can use a `Pricing::Sku` and set prices on that SKU, then provide the SKU in your customizations' attributes:

```ruby
module Workarea
  module Catalog
    class Customizations::Engraving < Customizations
      customized_fields :initials
      validates_presence_of :initials

      def attributes
        super.merge(pricing_sku: 'SKU123')
      end
    end
  end
end
```

If this SKU matches one in the database, the price will be applied to the item when ordered in the cart.
