spree_australia_settings
========================

Spree Settings for Australia

In config/initializers/spree.rb
```ruby
Spree.config do |config|
  config.currency = "AUD"
  config.default_country_id = 109 # check the country ID for your install
end
```

Add this in your db/seeds.rb
```ruby
 # Australian States and Territories
country = Spree::Country.find_by(name: 'Australia')
Spree::State.create!([
  { name: 'New South Wales', abbr: 'NSW', country: country },
  { name: 'South Australia', abbr: 'SA', country: country },
  { name: 'Queensland', abbr: 'QLD', country: country },
  { name: 'Australian Capital Territory', abbr: 'ACT', country: country },
  { name: 'Northern Territory', abbr: 'NT', country: country },
  { name: 'Tasmania', abbr: 'TAS', country: country },
  { name: 'Western Australia', abbr: 'WA', country: country }
])


 # Australian Zone
australia = Spree::Zone.create!(name: "Australia", description: "Australia", default_tax: true)


 # Australian Post Shipping if using the spree_aus_post_shipping gem
 
  begin
    australia = Spree::Zone.find_by_name!("Australia")
    # puts "found it"
  rescue ActiveRecord::RecordNotFound
    # puts "Couldn't find 'Australia' zone."
    exit
  end
  
  shipping_category = Spree::ShippingCategory.find_or_create_by_name!('Default')
  
  shipping_methods = [
    {
      :name => "Australia Post eParcel",
      :zones => [australia],
      :calculator => Spree::Calculator::Shipping::AusPostShipping.create!,
      :shipping_categories => [shipping_category]
    },
  ]
  
  shipping_methods.each do |shipping_method_attrs|
    ship_method = Spree::ShippingMethod.create!(shipping_method_attrs )
    ship_method.save!
  end 
 
```

Run
```ruby
rake db:seed
```
