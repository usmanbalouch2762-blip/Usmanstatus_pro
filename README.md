# Usmanstatus Pro

A comprehensive project for status tracking and zip code validation.

## Table of Contents
- [Installation](#installation)
- [Features](#features)
- [Zip Code Validation](#zip-code-validation)
- [Usage Examples](#usage-examples)

## Installation

Clone the repository and install dependencies:

```bash
git clone https://github.com/usmanbalouch2762-blip/Usmanstatus_pro.git
cd Usmanstatus_pro
unzip usmanstatus-pro.zip
pip install -r requirements.txt
```

## Features

- Status tracking and monitoring
- Zip code validation for multiple countries
- Email notifications
- Database integration

## Zip Code Validation

This project includes utilities for validating ZIP codes across different countries. Below are examples of how to use the zip code validation functions.

### US ZIP Code Validation

Validates US ZIP codes in both standard (5-digit) and extended (ZIP+4) formats.

```python
import re

def validate_us_zip(zip_code):
    """
    Validate US ZIP codes.
    Accepts formats: 12345 or 12345-6789
    """
    pattern = r'^\d{5}(-\d{4})?$'
    return bool(re.match(pattern, zip_code))

# Test cases
print(validate_us_zip("12345"))        # True
print(validate_us_zip("12345-6789"))   # True
print(validate_us_zip("1234"))         # False
print(validate_us_zip("12345-678"))    # False
```

### Canadian Postal Code Validation

Validates Canadian postal codes in the format `A1A 1A1` (letters and digits pattern).

```python
def validate_canadian_postal(zip_code):
    """
    Validate Canadian postal codes.
    Format: A1A 1A1 (letter, digit, letter, space, digit, letter, digit)
    """
    pattern = r'^[A-Za-z]\d[A-Za-z][ -]?\d[A-Za-z]\d$'
    return bool(re.match(pattern, zip_code))

# Test cases
print(validate_canadian_postal("K1A 0B1"))     # True
print(validate_canadian_postal("K1A0B1"))      # True
print(validate_canadian_postal("K1A-0B1"))     # True
print(validate_canadian_postal("12A 0B1"))     # False
```

### UK Postcode Validation

Validates UK postcodes with standard formats.

```python
def validate_uk_postcode(postcode):
    """
    Validate UK postcodes.
    Simplified pattern for common formats.
    """
    pattern = r'^[A-Z]{1,2}[0-9]{1,2}[A-Z]? ?[0-9][A-Z]{2}$'
    return bool(re.match(pattern, postcode.upper()))

# Test cases
print(validate_uk_postcode("SW1A 1AA"))   # True
print(validate_uk_postcode("M1 1AE"))     # True
print(validate_uk_postcode("B33 8TH"))    # True
print(validate_uk_postcode("123"))        # False
```

### Generic Multi-Country Zip Code Validator

A comprehensive function to validate zip codes for multiple countries.

```python
def validate_zip(zip_code, country='US'):
    """
    Validate ZIP codes for multiple countries.
    
    Args:
        zip_code (str): The zip code to validate
        country (str): Country code ('US', 'CA', 'UK', etc.)
    
    Returns:
        bool: True if valid, False otherwise
    """
    patterns = {
        'US': r'^\d{5}(-\d{4})?$',
        'CA': r'^[A-Za-z]\d[A-Za-z][ -]?\d[A-Za-z]\d$',
        'UK': r'^[A-Z]{1,2}[0-9]{1,2}[A-Z]? ?[0-9][A-Z]{2}$',
        'DE': r'^\d{5}$',
        'FR': r'^\d{5}$',
        'JP': r'^\d{3}-\d{4}$',
        'AU': r'^\d{4}$'
    }
    
    if country not in patterns:
        raise ValueError(f"Unsupported country: {country}")
    
    pattern = patterns[country]
    return bool(re.match(pattern, zip_code.upper() if country in ['UK'] else zip_code))

# Test cases
print(validate_zip("12345", "US"))           # True
print(validate_zip("K1A 0B1", "CA"))         # True
print(validate_zip("SW1A 1AA", "UK"))        # True
print(validate_zip("10115", "DE"))           # True
print(validate_zip("75001", "FR"))           # True
print(validate_zip("123-4567", "JP"))        # True
print(validate_zip("2000", "AU"))            # True
```

### Advanced Zip Code Validator Class

For more complex requirements, use this class-based approach:

```python
import re
from typing import Dict, Optional

class ZipCodeValidator:
    """
    A comprehensive ZIP code validator for multiple countries.
    """
    
    PATTERNS: Dict[str, str] = {
        'US': r'^\d{5}(-\d{4})?$',
        'CA': r'^[A-Za-z]\d[A-Za-z][ -]?\d[A-Za-z]\d$',
        'UK': r'^[A-Z]{1,2}[0-9]{1,2}[A-Z]? ?[0-9][A-Z]{2}$',
        'DE': r'^\d{5}$',
        'FR': r'^\d{5}$',
        'JP': r'^\d{3}-\d{4}$',
        'AU': r'^\d{4}$',
        'NZ': r'^\d{4}$',
        'IN': r'^\d{6}$'
    }
    
    @classmethod
    def is_valid(cls, zip_code: str, country: str = 'US') -> bool:
        """
        Validate a zip code for a specific country.
        
        Args:
            zip_code: The zip code to validate
            country: The country code
            
        Returns:
            True if valid, False otherwise
        """
        if country not in cls.PATTERNS:
            raise ValueError(f"Unsupported country: {country}")
        
        pattern = cls.PATTERNS[country]
        zip_upper = zip_code.upper() if country in ['UK'] else zip_code
        return bool(re.match(pattern, zip_upper))
    
    @classmethod
    def get_supported_countries(cls) -> list:
        """Get list of supported countries."""
        return list(cls.PATTERNS.keys())
    
    @classmethod
    def validate_multiple(cls, zip_codes: Dict[str, str]) -> Dict[str, bool]:
        """
        Validate multiple zip codes at once.
        
        Args:
            zip_codes: Dict of {country: zip_code}
            
        Returns:
            Dict of {country: is_valid}
        """
        return {country: cls.is_valid(code, country) 
                for country, code in zip_codes.items()}

# Usage
validator = ZipCodeValidator()

# Single validation
print(validator.is_valid("12345", "US"))      # True
print(validator.is_valid("K1A 0B1", "CA"))    # True

# Get supported countries
print(validator.get_supported_countries())    # All supported countries

# Validate multiple
results = validator.validate_multiple({
    'US': '12345',
    'CA': 'K1A 0B1',
    'UK': 'SW1A 1AA',
    'JP': '123-4567'
})
print(results)
```

## Usage Examples

### Basic Usage

```python
from usmanstatus_pro.validators import ZipCodeValidator

# Validate a single zip code
is_valid = ZipCodeValidator.is_valid("12345-6789", "US")
print(f"Valid ZIP Code: {is_valid}")

# Validate multiple zip codes
results = ZipCodeValidator.validate_multiple({
    'US': '90210',
    'CA': 'M5V 3A8',
    'UK': 'EC1A 1BB'
})

for country, result in results.items():
    print(f"{country}: {result}")
```

## Contributing

Contributions are welcome! Please fork the repository and submit a pull request.

## License

MIT License - see LICENSE file for details

## Support

For issues and questions, please open an issue on GitHub.
