+++
date = "2016-08-12T12:00:00-04:00"
draft = false
title = "Swift, UnitTemperature, And Humidex"

+++

Last week, I [wrote](/post/how-about-that-heat/) about weather apps that use [the Dark Sky forecast API](https://developer.forecast.io) for weather data lacking the Canadian [humidex](https://en.wikipedia.org/wiki/Humidex).

Those that do tend to be riddled with ads and all kinds of content that, well, I don't care about. And naturally, I started thinking about how I use weather apps. All I really want from my is a couple of things:

- Current conditions, including humidex/ windchill values;
- Forecast conditions with highs and lows for today and tomorrow, again including humidex and windchill;
- Probability of precipitation for the next hour, with alerts of impending rain.

Getting the actual forecast along with alerts is a solved problem, thanks to Forecast.io. Doing conversion and such between different units is also a solved problem, thanks to the new [Measurement](https://developer.apple.com/reference/foundation/nsmeasurement) class in Cocoa.

([Here's a great starter post on Meaurement](http://yannickloriot.com/2016/06/measurements-and-units-ios/))

So what would, say, a basic `CurrentConditions` object look like in my ideal app? Probably something like this:

```
import Foundation

struct CurrentConditions {
    var conditions: String
    var airTemperature: Measurement<UnitTemperature>
    var dewpoint: Measurement<UnitTemperature>
    
    var humidex: Measurement<UnitTemperature> {
        return Measurement(value: airTemperature.converted(to: UnitTemperature.celsius).value + 0.5555 * ((6.11 * exp(5417.7530 * ((1/273.16) - (1/dewpoint.converted(to: UnitTemperature.kelvin).value)))) - 10), unit: UnitTemperature.celsius)
    }
}
```

We initialize this strict with some descriptive weather conditions (e.g., "cloudy"), and values for the air (i.e., actual) temperature and dewpoint. It doesn't really matter what units we use when we pass the values in, because the calculated property `humidex` will convert them to Celsius and Kelvin respectively, before returning a value in Celsius.

Then you can simply add a switch in your weather app's settings asking the user if they prefer apparent temperature be calculated according to heat index or humidex.