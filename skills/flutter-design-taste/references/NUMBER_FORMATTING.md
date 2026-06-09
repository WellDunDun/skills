# Number Formatting

Use this reference when Flutter UI displays counts, money, large numbers, metrics, or any numeric value users read as quantity.

Sources:

- https://flutterpro.design/details/format-numbers-for-humans
- https://pub.dev/packages/intl

## Rule

If users read a number as "how much", format it. If they read it as "which one", leave it alone.

Format:

- Prices and balances.
- Counts and totals.
- Views, likes, downloads, followers.
- Percentages, measurements, and metrics.

Usually do not format:

- IDs.
- Confirmation codes.
- Phone-like strings.
- Version numbers.
- Order or invoice references when exact digits matter.

## Locale-aware counts

Use intl NumberFormat instead of hand-rolled commas:

    final String count = NumberFormat
        .decimalPatternDigits()
        .format(1234567);

This follows the user's region by default. Examples:

- en_US: 1,234,567
- de_DE: 1.234.567
- fr_FR: 1 234 567

Force locale or decimal count only when the product requires it:

    final String count = NumberFormat
        .decimalPatternDigits(
          locale: 'de_DE',
          decimalDigits: 2,
        )
        .format(1234567);

## Currency

Use simpleCurrency with a currency code so symbol placement and decimal count match the currency:

    final String price = NumberFormat
        .simpleCurrency(
          name: 'USD',
        )
        .format(1234.50);

Examples:

- en_US: $1,234.50
- de_DE: 1.234,50 $
- JPY: no decimal places.

## Compact numbers

Use compact for glanceable large values:

    final String views = NumberFormat
        .compact()
        .format(1234567);

    final String likes = NumberFormat
        .compact()
        .format(9800);

Use compactLong when the interface has room:

    final String views = NumberFormat
        .compactLong()
        .format(1234567);

## Extension pattern

Put common display rules in one place:

    extension NumX on num {
      String humanizedCount({
        int? decimalDigits,
      }) {
        return NumberFormat.decimalPatternDigits(
          decimalDigits: decimalDigits,
        ).format(this);
      }

      String humanizedCurrency(String code) {
        return NumberFormat.simpleCurrency(
          name: code,
        ).format(this);
      }

      String humanizedCompact() {
        return NumberFormat.compact().format(this);
      }
    }

## Review checklist

- Are quantities formatted with locale-aware separators?
- Are currencies formatted with the right symbol and decimal count?
- Are large counts compact only when exact precision is not needed?
- Are identifiers left unformatted?
- Are missing values handled by [NULL_DISPLAY.md](NULL_DISPLAY.md) before formatting?
