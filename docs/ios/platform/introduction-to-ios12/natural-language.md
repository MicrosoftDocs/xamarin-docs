---
title: "Using the Natural Language framework with Xamarin.iOS"
description: "This document describes the Natural Language Framework. Introduced in iOS 12, the Natural Language framework is the preferred iOS API to use for language recognition, parts of speech identification, and named entity recognition."
ms.service: xamarin
ms.assetid: 126C8764-F873-4EB9-98A3-D82AB5689111
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/20/2018
no-loc: [Objective-C]
---
# Using the Natural Language framework with Xamarin.iOS

Introduced in iOS 12, the Natural Language framework enables on-device
natural language processing. It supports language recognition,
tokenization, and tagging. Tokenization splits text into its component
words, sentences, or paragraphs; tagging identifies parts of speech,
people, places, and organizations.

The Natural Language framework can also use custom Core ML models to
classify and tag text in specialized contexts.

The [NSLinguisticTagger](xref:Foundation.NSLinguisticTagger)
class is still available. However, the Natural Language framework is the
preferred mechanism to use for Natural Language processing.

## Sample app: XamarinNL

To learn how to use the Natural Language framework with Xamarin.iOS, take
a look at the [XamarinNL sample app](/samples/xamarin/ios-samples/ios12-xamarinnl).
This sample app demonstrates how to use the Natural Language framework to:

- [Recognize languages](#recognizing-languages).
- [Tokenize text into words and sentences](#tokenizing-text-into-words-sentences-and-paragraphs).
- [Tag named entities and parts of speech](#tagging-named-entities-and-parts-of-speech).

## Recognizing languages

The **Recognizer** tab of the sample app demonstrates how to use an
[`NLLanguageRecognizer`](xref:NaturalLanguage.NLLanguageRecognizer)
to determine the language for a block of text.

> [!NOTE]
> Language recognition is a specific type of text classification. The
> Natural Language framework also supports custom text classification via
> developer-provided Core ML models. For more information, take a look at
> the [Introducing Natural Language Framework](https://developer.apple.com/videos/play/wwdc2018/713/)
> session from WWDC 2018.

### Dominant language

Tap the **Language** button to identify the dominant language in the user
input.

The `HandleDetermineLanguageButtonTap` method of the `LanguageRecognizerViewController` uses the
[`GetDominantLanguage`](xref:NaturalLanguage.NLLanguageRecognizer.GetDominantLanguage*)
method of an `NLLanguageRecognizer` to fetch the
[`NLLanguage`](xref:NaturalLanguage.NLLanguage)
for the primary language found in the text:

```csharp
partial void HandleDetermineLanguageButtonTap(UIButton sender)
{
    UserInput.ResignFirstResponder();
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        NLLanguage lang = NLLanguageRecognizer.GetDominantLanguage(UserInput.Text);
        DominantLanguageLabel.Text = lang.ToString();
    }
}
```

### Language probabilities

Tap the **Language probabilities** button to fetch a list of language
hypotheses for the user input.

The `HandleLanguageProbabilitiesButtonTap` method of the
`LanguageRecognizerViewController` class instantiates an
`NLLanguageRecognizer` and asks it to
[`Process`](xref:NaturalLanguage.NLLanguageRecognizer.Process*)
the user's text. It then calls the language recognizer's
[`GetNativeLanguageHypotheses`](xref:NaturalLanguage.NLLanguageRecognizer.GetNativeLanguageHypotheses*)
method, which fetches a dictionary of languages and associated
probabilities. The `LanguageRecognizerTableViewController` class then
renders these languages and probabilities.

```csharp
partial void HandleLanguageProbabilitiesButtonTap(UIButton sender)
{
    UserInput.ResignFirstResponder();
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        var recognizer = new NLLanguageRecognizer();
        recognizer.Process(UserInput.Text);
        NSDictionary<NSString, NSNumber> probabilities = recognizer.GetNativeLanguageHypotheses(10);
        PerformSegue(ShowLanguageProbabilitiesSegue, this);
    }
}
```

Potential `NLLanguage` values include:

- `Amharic`
- `Arabic`
- `Armenian`
- `Bengali`
- `Bulgarian`
- `Burmese`
- `Catalan`
- `Cherokee`
- `Croatian`
- `Czech`
- `Danish`
- `Dutch`
- `English`
- `Finnish`
- `French`
- `Georgian`
- `German`
- `Greek`
- `Gujarati`
- `Hebrew`
- `Hindi`
- `Hungarian`
- `Icelandic`
- `Indonesian`
- `Italian`
- `Japanese`
- `Kannada`
- `Khmer`
- `Korean`
- `Lao`
- `Malay`
- `Malayalam`
- `Marathi`
- `Mongolian`
- `Norwegian`
- `Oriya`
- `Persian`
- `Polish`
- `Portuguese`
- `Punjabi`
- `Romanian`
- `Russian`
- `SimplifiedChinese`
- `Sinhalese`
- `Slovak`
- `Spanish`
- `Swedish`
- `Tamil`
- `Telugu`
- `Thai`
- `Tibetan`
- `TraditionalChinese`
- `Turkish`
- `Ukrainian`
- `Undetermined`
- `Urdu`
- `Vietnamese`

A full list of supported languages is available as part of the
[`NLLanguage`](xref:NaturalLanguage.NLLanguage)
enum API documentation.

## Tokenizing text into words, sentences, and paragraphs

The **Tokenizer** tab of the sample app demonstrates how to separate
a block of text into its component words or sentences with an
[`NLTokenizer`](xref:NaturalLanguage.NLTokenizer).

Tap the **Words** or **Sentences** button to fetch a list of tokens. Each
token is associated with a word or sentence in the original text.

`ShowTokens` splits the user's input into tokens by calling the
[`GetTokens`](xref:NaturalLanguage.NLTokenizer.GetTokens*)
method of an `NLTokenizer`. This method returns an array of
[`NSValue`](xref:Foundation.NSValue)
objects, each wrapping an `NSRange` value corresponding to a token in
the original text.

```csharp
void ShowTokens(NLTokenUnit unit)
{
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        var tokenizer = new NLTokenizer(unit);
        tokenizer.String = UserInput.Text;
        var range = new NSRange(0, UserInput.Text.Length);
        NSValue[] tokens = tokenizer.GetTokens(range);
        PerformSegue(ShowTokensSegue, this);
    }
}
```

`LanguageTokenizerTableViewController` renders a single token in each table
cell. It extracts an `NSRange` from a token `NSValue`, finds the
corresponding string in the original text, and sets a label on the table
view cell:

```csharp
public override UITableViewCell GetCell(UITableView tableView, NSIndexPath indexPath)
{
    var cell = TableView.DequeueReusableCell(TokenCell);
    NSRange range = Tokens[indexPath.Row].RangeValue;
    cell.TextLabel.Text = Text.Substring((int)range.Location, (int)range.Length);
    return cell;
}
```

## Tagging named entities and parts of speech

The **Tagger** tab of the XamarinNL sample app demonstrates how to use the
[`NLTagger`](xref:NaturalLanguage.NLTagger)
class to associate categories with tokens of an input string.
The Natural Language framework includes built-in support for recognizing
people, places, organizations, and parts of speech.

> [!NOTE]
> The Natural Language framework also supports custom tagging schemes via
> developer-provided Core ML models. For more information, take a look at
> the [Introducing Natural Language Framework](https://developer.apple.com/videos/play/wwdc2018/713/)
> session from WWDC 2018.

Tap the **Named entities** or **Parts of speech** button to fetch:

- An array of `NSValue` objects, each wrapping an `NSRange` for a token
in the original text.
- An array of [`NLTag`](xref:NaturalLanguage.NLTag)
values – categories for the `NSValue` tokens at the same array index.

In `LanguageTaggerViewController`, `HandlePartsOfSpeechButtonTap` and
`HandleNamedEntitiesButtonTap` each call `ShowTags`, passing along an
[`NLTagScheme`](xref:NaturalLanguage.NLTagScheme) –
either `NLTagScheme.LexicalClass` (for parts of speech) or
`NLTagScheme.NameType` (for named entities).

`ShowTags` creates an `NLTagger`, instantiating it with an array of
`NLTagScheme` types for which it will be queried (in this case, only the
passed-in `NLTagScheme` value). It then uses the
[`GetTags`](xref:NaturalLanguage.NLTagger.GetTags*)
method on the `NLTagger` to determine the tags relevant to the text in the
user input.

```csharp
void ShowTags(NLTagScheme tagScheme)
{
    if (!String.IsNullOrWhiteSpace(UserInput.Text))
    {
        var tagger = new NLTagger(new NLTagScheme[] { tagScheme });
        var range = new NSRange(0, UserInput.Text.Length);
        tagger.String = UserInput.Text;

        NLTag[] tags = tagger.GetTags(range, NLTokenUnit.Word, tagScheme, NLTaggerOptions.OmitWhitespace, out NSValue[] ranges);
        NSValue[] tokenRanges = ranges;
        detailViewTitle = tagScheme == NLTagScheme.NameType ? "Named Entities" : "Parts of Speech";

        PerformSegue(ShowEntitiesSegue, this);
    }
}
```

The tags are then displayed in a table by the `LanguageTaggerTableViewController`.

Potential `NLTag` values include:

- `Adjective`
- `Adverb`
- `Classifier`
- `CloseParenthesis`
- `CloseQuote`
- `Conjunction`
- `Dash`
- `Determiner`
- `Idiom`
- `Interjection`
- `Noun`
- `Number`
- `OpenParenthesis`
- `OpenQuote`
- `OrganizationName`
- `Other`
- `OtherPunctuation`
- `OtherWhitespace`
- `OtherWord`
- `ParagraphBreak`
- `Particle`
- `PersonalName`
- `PlaceName`
- `Preposition`
- `Pronoun`
- `Punctuation`
- `SentenceTerminator`
- `Verb`
- `Whitespace`
- `Word`
- `WordJoiner`

A full list of supported tags is available as part of the
[`NLTag`](xref:NaturalLanguage.NLTag)
enum API documentation.

## Related links

- [Sample app – XamarinNL](/samples/xamarin/ios-samples/ios12-xamarinnl)
- [Introducing Natural Language Framework](https://developer.apple.com/videos/play/wwdc2018/713/)
- [Natural Language (Apple)](https://developer.apple.com/documentation/naturallanguage?language=objc)