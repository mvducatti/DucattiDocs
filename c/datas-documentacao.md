# Datas \(Documentação\)

## Microsoft

### Storage Strategies <a id="storage-strategies"></a>

According to the rules above, calculations on date values are only meaningful when you understand the time-zone information associated with the date value you are processing. This means that whether you are storing your value temporarily in a class member variable, or choosing to save the values you have gathered into a database or file, you as the programmer are responsible for applying a strategy that allows the associated time-zone information to be understood at a later time.

#### Best Practice \#1

> _When coding, store the time-zone information associated with a DateTime type in an adjunct variable._

An alternative, but less reliable, strategy is to make a steadfast rule that your stored dates will always be converted to a particular time-zone, such as GMT, prior to storage. This may seem sensible, and many teams can make it work. However, the lack of an overt signal that says that a particular DateTime column in a table in a database is in a specific time zone invariably leads to mistakes in interpretation in later iterations of a project.

A common strategy seen in an informal survey of different .NET-based applications is the desire to always have dates represented in universal \(GMT\) time. I say "desire" because this is not always practical. A case in point arises when serializing a class that has a DateTime member variable via a Web service. The reason is that a DateTime value type maps to a **XSD:DateTime** type \(as one would expect\), and the XSD type accommodates representing points in time in any time zone. We'll discuss the XML case later. More interestingly, a good percentage of these projects weren't actually achieving their goal, and were storing the date information in the server time zone without realizing it.

In these cases, an interesting fact is that the testers weren't seeing time conversion issues, so nobody had noticed that the code that was supposed to convert the local date information to UCT time was failing. In these specific cases, the data was later serialized via XML and was converted properly because the date information was in machine local time to start with.

Let's look at some **code that doesn't work**:Copy

```text
Dim d As DateTime
d = DateTime.Parse("Dec 03, 2003 12:00:00 PM") 'date assignment
d.ToUniversalTime()
```

The program above takes the value in variable **d** and saves it to a database, expecting the stored value to represent a UCT view of time. This example recognizes that the **Parse** method renders the result in local time unless some non-default culture is used as an optional argument to the Parse family of methods.

The previously shown code actually fails to convert the value in the DateTime variable **d** to universal time in the third line because, as written, the sample violates Rule \#4 \(the methods on the DateTime class do not convert the underlying value\). Note: this code was seen in an actual application that had been tested.

How did it pass? The applications involved were able to successfully compare the stored dates because, during testing, all of the data was coming from machines set to the same time-zone, so Rule \#1 was satisfied \(all dates being compared and calculated are localized to the same time-zone point of view\). The bug in this code is the kind that is hard to spot—a statement that executes but that doesn't do anything \(hint: the last statement in the example is a no-op as written\).

#### Best Practice \#2 <a id="best-practice-2"></a>

> _When testing, check to see that stored values represent the point-in-time value you intend in the time zone you intend_.

Fixing the code sample is easy:Copy

```text
Dim d As DateTime
d = DateTime.Parse("Dec 03, 2003 12:00:00 PM").ToUniversalTime()
```

Since the calculation methods associated with the DateTime value type never impact the underlying value, but instead return the result of the calculation, a program must remember to store the converted value \(if this is desired, of course\). Next we'll examine how even this seemingly proper calculation can fail to achieve the expected results in certain circumstances involving daylight savings time.

{% embed url="https://docs.microsoft.com/en-us/previous-versions/dotnet/articles/ms973825\(v=msdn.10\)?redirectedfrom=MSDN" %}

## C\#

### .ToUniversalTime\(\)

> DateTime.ToUniversalTime removes the timezone offset of the local timezone to normalize a DateTime to UTC. If you then use DateTime.ToLocalTime on the normalized value in another timezone, the timezone offset of that timezone will be added to the normalized value for correct representation in that timezone.

{% embed url="https://stackoverflow.com/questions/1201378/how-does-datetime-touniversaltime-work" %}

## Working with DateTime in ASP.NET Web API

{% embed url="https://carolynvanslyck.com/blog/2012/08/webapi-datetime/" %}

