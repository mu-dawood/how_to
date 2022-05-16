# How to use flutter localizations easy way


- add the configuration file in your root folder 
  >  the file called `l10n.yaml` 
  > ```yaml
  > arb-dir: lib/l10n  #the folder where localization files are
  > output-dir: lib/l10n/tr # the folder where generated classes will be , and it must be in your 
  > template-arb-file: app_ar.arb # the template file may as you can add only the ar texts and later translate others
  > output-localization-file: tr.dart # the file where the generated classes will be
  > output-class: TR # the class name
  > nullable-getter: false # if true, the TR.of(Context) will return null if the key is not found else it will throw an exception
  > synthetic-package: false # if true, the generated classes will be in the same package as the template file or it will be in flutter generated package the packge and its not easy to use if its not in the same package 
  >```
- add your translation files like `app_ar.arb` and `app_en.arb` these files acts like json files, 
  > note you can also name them any thing else but in this case you have to include locale key in each one of them
  > `"@@locale":"ar"`
- add flutter localizations packge
  > ```yaml
  >   flutter_localizations:
  >     sdk: flutter 
  > ```
- Now run this command `flutter gen-l10n` you will see the generated files in `l10n/tr/tr.dart` `l10n/tr/tr_ar.dart` `l10n/tr/tr_en.dart`
- We can now use our classes 
  + in our material widget 
    ```dart
         return const MaterialApp(
            localizationsDelegates: [
              ...GlobalMaterialLocalizations.delegates,
              TR.delegate,
            ],
            locale: Locale('ar'),
            supportedLocales: TR.supportedLocales,
          );
     ```
  + in our widgets
    ```dart
         Text(TR.of(context).appName)
     ```
     
## now what a bout the arb file tricks 
the arb file may be a simple json 

like this
```arb
"appName:":"My app name",
```

But there more things you can know about it

## place holders
   you can use place holders
   
```arb
"hello":"Hello {name}",
"@hello":{
  "placeholders":{
    "name": {}
  }
}
```

now the gerated class will contain this

```dart
  String hello(dynamic name)
  
  /// so you can use it
  Text(TR.of(context).hello("Mohammed")
```

what about if i need the name to be a string 

```arb
"hello":"Hello {name}",
"@hello":{
  "placeholders":{
    "name": {
       "type": "String",
    }
  }
}
```

What about date time and doubles

If you used a placehoder which in type of DateTime or double you need to add also its format, see intl package formates

```arb
  "printDate": "Hey the current date is {date}",
  "@printDate": {
    "placeholders": {
      "date": {
        "type": "DateTime",
        "format": "yMEd"
      }
    }
  },
  "printTime": "Hey the current time is {date}",
  "@printTime": {
    "placeholders": {
      "date": {
        "type": "DateTime",
        "format": "jm"
      }
    }
  },
```

## plurals 

plurals are agood way to work with counts

```arb
"hours": "{count,plural, =0{Day not started} =1{One hour} =2{Two hours}  few{{count} hours} many{{count} hours} other{{count} years}}",
"@hours":{
    "placeholders": {
      "count": {}
    }
}
```

you can know more about plurals in localizely https://localizely.com/language-plural-rules/
but note that zero is 0= ,one is 1= , two is 2=


see also this for other features like select https://localizely.com/flutter-arb/



