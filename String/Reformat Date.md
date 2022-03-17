# Reformat Date

Given a date string in the form Day Month Year, where:

* Day is in the set {"1st", "2nd", "3rd", "4th", ..., "30th", "31st"}.
* Month is in the set {"Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"}.
* Year is in the range [1900, 2100].

Convert the date string to the format YYYY-MM-DD, where:

* YYYY denotes the 4 digit year.
* MM denotes the 2 digit month.
* DD denotes the 2 digit day.

## My solution:

```Java
class Solution {
    public String reformatDate(String date) {
        List<String> MONTHS = List.of("Jan", "Feb", "Mar", "Apr",
                                     "May", "Jun", "Jul", "Aug",
                                     "Sep", "Oct", "Nov", "Dec");
        
        //[0] = day
        //[1] = month
        //[2] = year
        String[] dateParts = date.split(" ");
        StringBuilder sb = new StringBuilder(dateParts[2]);//year
        sb.append("-" + String.format("%02d", MONTHS.indexOf(dateParts[1]) + 1));//month
        int day = Integer.parseInt(dateParts[0].substring(0, dateParts[0].length() - 2));
        sb.append("-" + String.format("%02d", day));//day
        return sb.toString();
    }
}
```

## My other solution (used a Map to store months just because; pulling the month out of a Map should be faster than traversing a List for the index anyway):

```Java
class Solution {
    public String reformatDate(String date) {
        Map<String, String> months = Stream.of(new String[][] {
            {"Jan", "01"},
            {"Feb", "02"},
            {"Mar", "03"},
            {"Apr", "04"},
            {"May", "05"},
            {"Jun", "06"},
            {"Jul", "07"},
            {"Aug", "08"},
            {"Sep", "09"},
            {"Oct", "10"},
            {"Nov", "11"},
            {"Dec", "12"},
        }).collect(Collectors.toMap(month -> month[0], month -> month[1]));
        
        String[] dateParts = date.split(" ");
        StringBuilder sb = new StringBuilder(dateParts[2]);//year
        sb.append("-" + months.get(dateParts[1]));//month
        int day = Integer.parseInt(dateParts[0].substring(0, dateParts[0].length() - 2));
        sb.append("-" + String.format("%02d", day));
        return sb.toString();
    }
}
```
