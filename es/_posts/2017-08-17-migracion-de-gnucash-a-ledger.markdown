---
---

En mi intento de llevar la contabilidad estuve usando [GNUCash][] y la verdad
estoy muy contento con como va, tiene algunas funciones que están muy buenas y
aprendí bastante de contabilidad en el proceso.

Ahora me encontré con [ledger][], que hace un planteo muy interesante sobre
como llevar la contabilidad y me dieron ganas de probarlo. Ahí me encontré con
que GNUCash tiene muchas funciones de importación de datos, pero muy pocas de
exportación, de forma que este proceso es bastante molesto.

Lo que hice fue hacer un script en ruby que toma la base de datos de GNUCash y
genera el archivo de ledger.

```ruby
# gnucash2ledger.rb
#
# Ejemplo de uso:
#   ruby gnucash2ledger.rb < origen.gnucash > destino.ledger

require 'nokogiri'
require 'date'
require 'bigdecimal'

gnucash_file = Nokogiri::XML(ARGF)
gnucash_file.remove_namespaces!

transactions = gnucash_file.xpath('//transaction')

class Account
  attr_reader :name, :currency
  def initialize name, currency
    @name = name
    @currency = currency
  end
end

accounts = Hash.new do |memo, guid|
  account = gnucash_file.xpath("//account/id[text() = '#{ guid }']/..")
  parent = account.xpath('parent')
  if parent.empty?
    name = nil
  else
    parent_account = accounts[parent.inner_text]
    name = [parent_account.name, account.xpath('name').inner_text].compact.join(':')
  end
  currency = account.xpath('commodity/id').inner_text
  account = Account.new name, currency
  memo[guid] = account
end

transactions.each do |transaction|
  date = Date.parse transaction.xpath('date-posted/date').first
  description = transaction.xpath('description').first.content
  currency = transaction.xpath('currency/id').first.content

  splits = transaction.xpath 'splits/split' 

  puts "#{ date } #{ description }"
  splits.each do |split|
    account_id = split.xpath('account').inner_text
    account = accounts[account_id]
    ammount = split.xpath('quantity').inner_text.to_r.to_f
    puts "  %-35s % 4.2f %s" % [account.name, ammount, account.currency]
  end
  puts
end
```

El script es bastante rústico, pero es comprensible, de forma que se puede
adaptar según las necesidades.

 [GNUCash]: http://gnucash.org/
 [ledger]: http://ledger-cli.org/ 

