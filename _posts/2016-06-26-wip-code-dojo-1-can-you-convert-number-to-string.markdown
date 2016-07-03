---
published: true
title: Code Dojo1 - Can You Convert Number To String?
layout: post
---
### Think of the possible usecase before implementing

To begin with, let's think about what type of numbers there are in the world to design the program.

#### Type of languages
- English such as one, two, three, four...
- Japanese such as 一、二、三、四...
- Roman Numeral such as I, II, III, IV...

#### Type of numbers
- negative and positive integer
- zero
- floating point
- one..ten, eleven, twenty, thirty, hundred
- thousand, million, billion, trillion etc

#### Largest and the smallest numbers
We need to handle the those numbers. Let's think about possible ways.
- Set the limit
- Allow to work with the infinite parameter but how?(It sounds hard though.)

#### Unexpected parameters
Next, how do we handle the unexpected parameters? What type of it could be passed? We could list the possible unexpected parameters like the below. 
- Multibyte string such as 'あ'
- Not a number such as 'hoge'
- Strings which contains comma such as '1,000,000'
- Strings which contains whitespace such as '1 000 000'
- Strings which contains both string and number such as '1000円'

### Organize the specifications
Now that we have though possible usecases as much as we can, we could make a specification of our app. I decided the easier and leaner one.

- The app is executable by CLI.
- Allow to convert number to word which is in only English.
- Allow to convert zero, negative, float and large number by centillion.
- Dont't allow user to pass non number text but allow to pass comma and whitespaces.

### Let's write the codes!

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
      it { expect(subject).to eq('negative one') }
    end

    context "Type String " do
      let(:number){ "-1" }
      it { expect{subject}.not_to raise_error }
      it { expect(subject).to eq('negative one') }
    end

  end

  context "Zero" do
    let(:number){ 0 }
    it { expect{subject}.not_to raise_error }
    it { expect(subject).to eq("zero") }
  end

  context "Floating point" do
    let(:number){ 1000.11 }
    it { expect{subject}.not_to raise_error }
    it { expect(subject).to eq('one thousand point one one') }
  end

  (1..10).each do |num|
    context "Range 1-10: #{num}" do
      let(:number){ num }
      it { expect{subject}.not_to raise_error }
      it { expect(subject).to eq(SAMPLE_NUMBERS[num.to_s]) }
    end
  end

  (11..100).each do |num|
    context "Range 11-100: #{num}" do
      let(:number){ num }
      it { expect{subject}.not_to raise_error }
      it { expect(subject).to eq(SAMPLE_NUMBERS[num.to_s]) }
    end
  end

  context "Large number" do
    context "Just one thousand" do
      let(:number){ "1,000" }
      it { expect{subject}.not_to raise_error }
      it { expect(subject).to eq('one thousand') }
    end

    context "More than one thousand" do
      let(:number){ "1,123" }
      it { expect{subject}.not_to raise_error }
      it { expect(subject).to eq('one thousand one hundred and twenty-three') }
    end

    context "More than one million" do
      let(:number){ "1,123,456" }
      it { expect{subject}.not_to raise_error }
      it { expect(subject).to eq('one million one hundred and twenty-three thousand four hundred and fifty-six') }
    end

    context "More than one billion" do
      let(:number){ "1,123,456,789" }
      it { expect{subject}.not_to raise_error }
      it { expect(subject).to eq('one billion one hundred and twenty-three million four hundred and fifty-six thousand seven hundred and eighty-nine') }
    end

    context "More than one trillion" do
      let(:number){ "1,123,456,789,012" }
      it { expect{subject}.not_to raise_error }
      it { expect(subject).to eq('one trillion one hundred and twenty-three billion four hundred and fifty-six million seven hundred and eighty-nine thousand twelve') }
    end

    context "More than one centillion" do
      let(:number){ 7128895353753512801851213839122324656430517127764242492772122014363994162410568756562761573538649597748463505127063089169061629466715041366197187934797638009975342907981315112215903509554633328742826248199166160019498345725451164673477322579175931292781354746911215934169601043364358422156689012325474774 }
      it { expect{subject}.not_to raise_error }
      it { expect(subject).to eq('seven centillion one hundred and twenty-eight novemnonagintillion eight hundred and ninety-five octononagintillion three hundred and fifty-three septennonagintillion seven hundred and fifty-three sexnonagintillion five hundred and twelve quinnonagintillion eight hundred and one quattuornonagintillion eight hundred and fifty-one trenonagintillion two hundred and thirteen duononagintillion eight hundred and thirty-nine unnonagintillion one hundred and twenty-two nonagintillion three hundred and twenty-four novemoctogintillion six hundred and fifty-six octooctogintillion four hundred and thirty septoctogintillion five hundred and seventeen sexoctogintillion one hundred and twenty-seven quinoctogintillion seven hundred and sixty-four quattuoroctogintillion two hundred and forty-two treoctogintillion four hundred and ninety-two duooctogintillion seven hundred and seventy-two unoctogintillion one hundred and twenty-two octogintillion fourteen novemseptuagintillion three hundred and sixty-three octoseptuagintillion nine hundred and ninety-four septenseptuagintillion one hundred and sixty-two sexseptuagintillion four hundred and ten quinseptuagintillion five hundred and sixty-eight quattuorseptuagintillion seven hundred and fifty-six treseptuagintillion five hundred and sixty-two duoseptuagintillion seven hundred and sixty-one unseptuagintillion five hundred and seventy-three septuagintillion five hundred and thirty-eight novemsexagintillion six hundred and forty-nine octosexagintillion five hundred and ninety-seven septensexagintillion seven hundred and forty-eight sexsexagintillion four hundred and sixty-three quinsexagintillion five hundred and five quattuorsexagintillion one hundred and twenty-seven tresexagintillion sixty-three duosexagintillion eighty-nine unsexagintillion one hundred and sixty-nine sexagintillion sixty-one novemquinquagintillion six hundred and twenty-nine octoquinquagintillion four hundred and sixty-six septenquinquagintillion seven hundred and fifteen sexquinquagintillion forty-one quinquinquagintillion three hundred and sixty-six quattuorquinquagintillion one hundred and ninety-seven trequinquagintillion one hundred and eighty-seven duoquinquagintillion nine hundred and thirty-four unquinquagintillion seven hundred and ninety-seven quinquagintillion six hundred and thirty-eight novemquadragintillion nine octoquadragintillion nine hundred and seventy-five septenquadragintillion three hundred and forty-two sexquadragintillion nine hundred and seven quinquadragintillion nine hundred and eighty-one quattuorquadragintillion three hundred and fifteen trequadragintillion one hundred and twelve duoquadragintillion two hundred and fifteen unquadragintillion nine hundred and three quadragintillion five hundred and nine novemtrigintillion five hundred and fifty-four octotrigintillion six hundred and thirty-three septentrigintillion three hundred and twenty-eight sextrigintillion seven hundred and forty-two quintrigintillion eight hundred and twenty-six quattuortrigintillion two hundred and forty-eight tretrigintillion one hundred and ninety-nine duotrigintillion one hundred and sixty-six untrigintillion one hundred and sixty trigintillion nineteen novemvigintillion four hundred and ninety-eight octovigintillion three hundred and forty-five septenvigintillion seven hundred and twenty-five sexvigintillion four hundred and fifty-one quinvigintillion one hundred and sixty-four quattuorvigintillion six hundred and seventy-three trevigintillion four hundred and seventy-seven duovigintillion three hundred and twenty-two unvigintillion five hundred and seventy-nine vigintillion one hundred and seventy-five novemdecillion nine hundred and thirty-one octodecillion two hundred and ninety-two septendecillion seven hundred and eighty-one sexdecillion three hundred and fifty-four quindecillion seven hundred and forty-six quattuordecillion nine hundred and eleven tredecillion two hundred and fifteen duodecillion nine hundred and thirty-four undecillion one hundred and sixty-nine decillion six hundred and one nonillion forty-three octillion three hundred and sixty-four septillion three hundred and fifty-eight sextillion four hundred and twenty-two quintillion one hundred and fifty-six quadrillion six hundred and eighty-nine trillion twelve billion three hundred and twenty-five million four hundred and seventy-four thousand seven hundred and seventy-four') }
    end

  end

```


##### Implement the conversion to pass the tests

```
  def convert(num)
    negative = false
    valid_num = validate(num)
    return 'zero' if valid_num.zero?
    if valid_num < 0
      valid_num = valid_num.abs
      negative = true
    end

    processing_num = valid_num
    stack = []
    i = 0
    until processing_num.zero?
      word = ''
      processing_num, remainder = processing_num.divmod(1000)
      word << BIG_NUMBERS[i] if processing_num.nonzero? && processing_num != 1000
      word << (word.empty? ? '' : ' ') + "#{SUB_ONE_THOUSAND[remainder]}" if remainder.nonzero? && remainder.is_a?(Integer)
      stack << word
      i = i.succ
    end
    words = (negative ? 'negative ' : '') \
      + stack.reverse.join(' ') \
      + (valid_num.is_a?(Float) ? ' point ' + valid_num.to_s.split('.')[1].split(//).map{|e| SUB_ONE_THOUSAND[e.to_i]}.join(' ') : '')
  end

```

### Performance tuning
- benchmark to tune the perfomance

```
require 'benchmark'
require_relative 'lib/number2word'

include Number2Word

Benchmark.benchmark do |x|
  x.report(:convert_big_num) do
    100.times { |n| convert(rand(10**303..10**304)) }
  end
end
```

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
- [Benchmark](http://ruby-doc.org/stdlib-1.9.3/libdoc/benchmark/rdoc/Benchmark.html)
