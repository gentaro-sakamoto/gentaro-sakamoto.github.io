---
published: true
title: (WIP) Code Dojo1 - Can You Convert Number To String?
layout: post
---
### Design the program before implementing

To begin with, let's think about what type of numbers there are in the world to design the program.

#### Type of numbers
- negative and positive integer
- zero
- floating point
- one..ten, eleven, twenty, thirty, hundred
- thousand, million, billion, trillion etc

Next, how do we handle the largest and the smallest number? Let's think about possible ways.

#### Ideas handling with the largest and the smallest number
- Set the limit
- Allow to work with the infinite parameter but how? It sounds hard though.

Next, how do we handle the unexpected parameters? What type of it could be passed?

#### Unexpected parameters
- Multibyte string such as 'あ'
- Not a number such as 'hoge'
- Strings which contains comma such as '1,000,000'
- Strings which contains whitespace such as '1 000 000'
- Strings which contains both string and number such as '1000円'

We could list the possible unexpected parameters like the above. This time, if the application receive just a string or the number containing string, then let's raise exception with messages which tell the user what happend. And, if the application receive a number containing whitespaces or commas, let's remove them and convert the valid number to the word. That's our validation specification.

### Do the coding

#### Validation
##### Write test first and make sure it fails

*spec/number2word_spec.rb*

```
require 'number2word'

describe "Number2Word" do
  include Number2Word
  subject{ convert(number) }
  context "unexpected paramters passed" do
    context "without any parameters" do
      let(:number){ nil }
      it { expect{subject}.to raise_error("You need to pass a number") }
    end

    context "with multibyte string" do
      let(:number){ 'あ' }
      it { expect{subject}.to raise_error("You can only pass arabic numerals") }
    end

    context "with single byte string" do
      let(:number){ 'hoge' }
      it { expect{subject}.to raise_error("You can only pass arabic numerals") }
    end

    context "with string containing both string and number" do
      let(:number){ '10000円' }
      it { expect{subject}.to raise_error("You can only pass arabic numerals") }
    end

    context "with string containing commas" do
      let(:number){ '1,000,000' }
      it { expect{subject}.not_to raise_error }
    end

    context "with string containing whitespaces" do
      let(:number){ '1 000 000' }
      it { expect{subject}.not_to raise_error }
    end

  end
end
```

##### Implement the validation to pass the tests

*lib/number2word.rb*

```
module Number2Word
  def convert(num)
    valid_num = validate(num)
    puts valid_num
  end

  def validate(num)
    raise 'You need to pass a number' if num.nil? || num.empty?
    num_without_whtiespace_or_comma = num.gsub(/[\s,]/ ,"")
    raise 'You can only pass arabic numerals' unless num_without_whtiespace_or_comma =~ /\A[-+]?[0-9]*\.?[0-9]+\Z/
  end
end
```

Then run the spec and make sure it passed.

```
dev-mac:number2word gentaro$ rspec spec/
....
.
.

Finished in 0.00716 seconds (files took 0.1196 seconds to load)
6 examples, 0 failures

```

#### Conversion number to word

##### Write test first and make sure it fails

```
context "Negative" do
    context "Type Fixnum " do
      let(:number){ -1 }
      it { expect{subject}.not_to raise_error }
    end

    context "Type String " do
      let(:number){ "-1" }
      it { expect{subject}.not_to raise_error }
    end

  end

  context "Zero" do
    let(:number){ 0 }
    it { expect{subject}.not_to raise_error }
  end

  context "Floating point" do
    let(:number){ 1000.11 }
    it { expect{subject}.not_to raise_error }
  end

  (1..10).each do |num|
    context "Range 1-10: #{num}" do
      let(:number){ num }
      it { expect{subject}.not_to raise_error }
    end
  end

  (11..100).each do |num|
    context "Range 11-100: #{num}" do
      let(:number){ num }
      it { expect{subject}.not_to raise_error }
    end
  end

  context "Large number" do
    context "More than one thousand" do
      let(:number){ 1,123 }
      it { expect{subject}.not_to raise_error }
    end

    context "More than one thousand" do
      let(:number){ 1,123 }
      it { expect{subject}.not_to raise_error }
    end

    context "More than one million" do
      let(:number){ 1,123,456 }
      it { expect{subject}.not_to raise_error }
    end

    context "More than one billion" do
      let(:number){ 1,123,456,789 }
      it { expect{subject}.not_to raise_error }
    end

    context "More than one trillion" do
      let(:number){ 1,123,456,789,012 }
      it { expect{subject}.not_to raise_error }
    end

    context "More than one centillion" do
      let(:number){ 1480618280045048488315358590684251505011921155404500899903072688797535200146390206121481470497873024987128876838600494835763269731405991593515845435981815326803392562826356453809261018413280605369122309536165893153748009139737779101683887892453159966154221795175433994469581972750121141578558011435313335 }
      it { expect{subject}.not_to raise_error }
    end

  end
```

##### Implement the conversion to pass the tests
TBD

### Performance tuning
- bench mark

TBD

### Tips

#### Generating a large number

```
dev-mac:gentaro-sakamoto.github.io gentaro$ ruby -e "puts rand(10**303..10**304)"
7128895353753512801851213839122324656430517127764242492772122014363994162410568756562761573538649597748463505127063089169061629466715041366197187934797638009975342907981315112215903509554633328742826248199166160019498345725451164673477322579175931292781354746911215934169601043364358422156689012325474774
```


### Sample Code
[gentaro-sakamoto/number2word](https://github.com/gentaro-sakamoto/number2word)


### References
- [radar/humanize](https://github.com/radar/humanize)
- [Number Names](http://www.thealmightyguru.com/Pointless/BigNumbers.html)
