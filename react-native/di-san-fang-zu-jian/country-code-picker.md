# Country Code Picker

[https://github.com/xcarpentier/react-native-country-picker-modal](https://github.com/xcarpentier/react-native-country-picker-modal)

```javascript
import CountryPicker from 'react-native-country-picker-modal';

<CountryPicker
  withFilter
  withFlag
  withCountryNameButton
  withAlphaFilter
  withCallingCode
  withEmoji
  placeholder={
    <Text
      style={{
        borderWidth: 1,
        borderColor: '#e6e6e6',
      }}>
      +{countryCode}
    </Text>
  }
  onSelect={handleCountrySelect}
/>
```

