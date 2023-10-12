# CamelCaseSFSymbols
A simple swift struct containing all 5296 SF Symbol 5 icons with CamelCase names making it easy and safe to use.

## Installation
Simply add the .swift file to your project and start typing.
## Example usage
Anywhere you can type the name of an SF Symbol you can use this instead. You'll even get suggestions in xcode while typing. To get a symbol start writing "SF." and the name of the icon. Numbers are written out ("creditcard.and.123" -> CreditcardAndOneHundredTwentyThree):

```bash
Label("Featured", systemImage: SF.PlaySquareStackFill)
```
```bash
Image(systemName: audioPlayer.isPlaying ? SF.PauseFill : SF.PlayFill)
```
```bash
Image(systemName: SF.CreditcardAndOneHundredTwentyThree)
```
## Make it yourself
Here is the function i used to create the struct. Run it in swift playgrounds and the struct will pe printed in the console.

Replace symbolNames with a string containing all the SF Symbol names. You can get these names by using the SF Symbols app, selecting all symbols and copying their names.
### It's not perfect
The function incorrectly translates some names that usually start with a number or a number with a leading zero, so some manual tweaking of the final struct is needed, but not much.

```bash
//Generate SF struct
let symbolNames = "square.and.arrow.up square.and.arrow.up.fill square.and.arrow.up.circle etc"

func generateStruct(from symbolNames: String) -> String {
    let symbolArray = symbolNames.components(separatedBy: " ")

    let structDefinition = symbolArray.map { symbol in
        let camelCaseName = convertToCamelCase(symbol)
        return "    let \(camelCaseName) = \"\(symbol)\""
    }.joined(separator: "\n")

    let generatedStruct = """
struct SF {
\(structDefinition)
}
"""

    return generatedStruct
}

func convertToCamelCase(_ input: String) -> String {
    let components = input.components(separatedBy: CharacterSet.alphanumerics.inverted)
    let camelCaseWords = components.map { component -> String in
        if let number = Int(component) {
            return convertNumberToWords(number)
        } else {
            return component.prefix(1).capitalized + component.dropFirst()
        }
    }
    return camelCaseWords.joined()
}

func convertNumberToWords(_ number: Int) -> String {
    let numberFormatter = NumberFormatter()
    numberFormatter.numberStyle = .spellOut
    
    guard let word = numberFormatter.string(from: NSNumber(value: number)) else {
        return "\(number)"
    }
    let components = word.components(separatedBy: CharacterSet(charactersIn: " -"))

    let concatenatedString = components.map { $0.lowercased().capitalized }.joined()

    return concatenatedString
}


let generatedStruct = generateStruct(from: symbolNames)
print(generatedStruct)
```
