# ConvertImageToHex
Convert the image to hexadecimal to send the image to e-paper

## Conversion Order
```swift
// 0. hex로 변환할 이미지
var image = UIImage(named: "sample")
// 1. 이미지를 원하는 사이즈로 변경한다.
// sample size = 250*128
let resizedImage = image?.imageResizing
// 2. 이미지 컬러를 흑백으로 변경한다.
let blackAndWhiteImage = resizedImage?.convertToBlackAndWhite
// 3. 변환한 이미지에서 binary를 추출한다.
let binary = extractBinary(from: blackAndWhiteImage)
// 4. binary string을 hex로 변환한다.
let hexString = convertToHexString(with: binary)
// 5. 변환된 hex를 데이터로 변경한다.
let hexData = hexString.hexaData
// 6. 데이터를 전송한다.
```

## Extension
### String+Ext
1. string to hexaData
```swift
var hexaData: Data { .init(hexa) }
private var hexa: UnfoldSequence<UInt8, Index> {
    sequence(state: startIndex) { startIndex in
        guard startIndex < self.endIndex else { return nil }
        let endIndex = self.index(startIndex, offsetBy: 2, limitedBy: self.endIndex) ?? self.endIndex
        defer { startIndex = endIndex }
        let binary = self[startIndex..<endIndex]
        return UInt8(binary, radix: 16)
    }
}
```

2. subString
```swift
func substring(from: Int, to: Int) -> String {
    guard from < self.count,
          to <= self.count,
          to >= 0,
          (to - from) >= 0 else {
              return "out of range"
          }

    let startIndex = index(self.startIndex, offsetBy: from)
    let endIndex = index(self.startIndex, offsetBy: to + 1)
    return String(self[startIndex ..< endIndex])
}
```

3. padding to left
```swift
func paddingToLeft(toLength: Int, withPad: String, startingAt: Int) -> String {
    guard count < toLength else { return self }
    return String(
        self.padding(toLength: toLength,
                     withPad: withPad,
                     startingAt: startingAt).reversed()
    )
}
```
