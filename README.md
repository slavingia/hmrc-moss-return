# HMRC VAT MOSS Return Generator

This library helps you generate a VAT MOSS Return from Ruby. In HMRC's infinate
wisdom, they have decided that users must submit returns in a formatted ODS file.
(unlike the EC Sales List, where they allow you submit CSV).

## Usage

```ruby
require 'hmrc_moss'

# Create a new return and provide the period which you are reporting for.
moss_return = HMRCMOSS::Return.new('Q1/2015')

# Add any sales which you made FROM the UK to other member states.
moss_return.supplies_from_uk do
  line 'AT', :type => 'standard', :rate => 20, :value => 2000, :amount => 10
  line 'DE', :type => 'standard', :rate => 20, :value => 2000, :amount => 10
  line 'FR', :type => 'standard', :rate => 20, :value => 2000, :amount => 10
  line 'NL', :type => 'standard', :rate => 20, :value => 2000, :amount => 10
end

# Add any sales made from fixed establishments in other member states.
moss_return.supplies_from_outside_uk do
  line '123123123', 'AT', :type => 'standard', :rate => 20, :value => 2000, :amount => 10
  line '123123123', 'DE', :type => 'standard', :rate => 20, :value => 2000, :amount => 10
  line '123123123', 'FR', :type => 'standard', :rate => 20, :value => 2000, :amount => 10
  line '123123123', 'NL', :type => 'standard', :rate => 20, :value => 2000, :amount => 10
end

# Save the restuling file
moss_return.ods_file.save('path/to/return.ods')
```

## Output

The resulting ODS file is based on the template provided by HMRC.

![Example](https://s.adamcooke.io/15/q1GjF3.png)

## Important Notes

* No validation is carried out on the data you provide. You should ensure it is all correct.
  The resulting sheet will simply include the data you provide.
* A file generated by this has not yet been submitted to HMRC however we have no reason
  to expect it to be rejected based on the method used to generate the file.
* The author accepts not responsible whatsoever for any failures or mis-reporting. You
  are expected to fully review all output before submitting to HMRC.
