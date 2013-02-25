---
layout: page
title: AirfoilJSON *DRAFT* Specification
---

AirfoilJSON is a format based on JavaScript Object Notation (JSON) that aims to faithfully represent the information contained in a [UIUC airfoil data file](http://www.ae.illinois.edu/m-selig/ads.html).  You can think of it as "[GeoJSON](http://www.geojson.org/) for airfoils".  An AirfoilJSON object must also be a valid JSON object.  It is possible that a web service might return a list of AirfoilJSON objects.

### Examples ###

An example of an Airfoil (NACA 63-412) encoded as AirfoilJSON:

{% highlight javascript %}
{
    "type" : "Airfoil",
    "name" : "NACA 63-412 AIRFOIL",
    "coordinates" : [
        {"x" : 1.000000, "y" : 0.000000},
        {"x" : 0.950230, "y" : 0.008810},
        {"x" : 0.900490, "y" : 0.017390},
        {"x" : 0.850700, "y" : 0.026180},
        {"x" : 0.800840, "y" : 0.034920},
        {"x" : 0.750890, "y" : 0.043440},
        {"x" : 0.700870, "y" : 0.051530},
        {"x" : 0.650760, "y" : 0.058990},
        {"x" : 0.600570, "y" : 0.065620},
        {"x" : 0.550310, "y" : 0.071250},
        {"x" : 0.500000, "y" : 0.075670},
        {"x" : 0.449640, "y" : 0.078940},
        {"x" : 0.399240, "y" : 0.080620},
        {"x" : 0.348820, "y" : 0.080590},
        {"x" : 0.298400, "y" : 0.078720},
        {"x" : 0.248000, "y" : 0.074990},
        {"x" : 0.197650, "y" : 0.069290},
        {"x" : 0.147350, "y" : 0.061380},
        {"x" : 0.097180, "y" : 0.050630},
        {"x" : 0.072180, "y" : 0.043790},
        {"x" : 0.047270, "y" : 0.035440},
        {"x" : 0.022570, "y" : 0.024600},
        {"x" : 0.010410, "y" : 0.017190},
        {"x" : 0.005670, "y" : 0.013200},
        {"x" : 0.003360, "y" : 0.010710},
        {"x" : 0.000000, "y" : 0.000000},
        {"x" : 0.006640, "y" : -0.008710},
        {"x" : 0.009330, "y" : -0.010400},
        {"x" : 0.014590, "y" : -0.012910},
        {"x" : 0.027430, "y" : -0.017160},
        {"x" : 0.052730, "y" : -0.022800},
        {"x" : 0.077820, "y" : -0.026850},
        {"x" : 0.102820, "y" : -0.029950},
        {"x" : 0.152650, "y" : -0.034460},
        {"x" : 0.202350, "y" : -0.037450},
        {"x" : 0.252000, "y" : -0.039190},
        {"x" : 0.301600, "y" : -0.039840},
        {"x" : 0.351118, "y" : -0.039390},
        {"x" : 0.400760, "y" : -0.037780},
        {"x" : 0.450350, "y" : -0.035140},
        {"x" : 0.500000, "y" : -0.031640},
        {"x" : 0.549690, "y" : -0.027450},
        {"x" : 0.599430, "y" : -0.022780},
        {"x" : 0.649240, "y" : -0.017990},
        {"x" : 0.699130, "y" : -0.012650},
        {"x" : 0.749110, "y" : -0.007640},
        {"x" : 0.799160, "y" : -0.003080},
        {"x" : 0.849300, "y" : 0.000740},
        {"x" : 0.899510, "y" : 0.003290},
        {"x" : 0.949770, "y" : 0.003300}
    ]

}
{% endhighlight %}

An example of a collection of flat plate airfoils encoded as AirfoilJSON:

{% highlight javascript %}
{
    "type" : "AirfoilCollection",
    "airfoils" : [
        {
            "type" : "Airfoil",
            "name" : "A very flat plate",
            "coordinates" : [
                {"x" : 1.000000, "y" : 0.000000},
                {"x" : 0.000000, "y" : 0.000000},
                {"x" : 1.000000, "y" : 0.000000}
            ]
        },
        {
            "type" : "Airfoil",
            "name" : "A very flat plate with some thickness",
            "coordinates" : [
                {"x" : 1.000000, "y" : 0.100000},
                {"x" : 0.000000, "y" : 0.100000},
                {"x" : 0.000000, "y" : -0.100000},
                {"x" : 1.000000, "y" : -0.100000}
            ]
        }
    ]
}
{% endhighlight %}

An example of an Airfoil with additional members encoded as AirfoilJSON:

{% highlight javascript %}
{
    "type" : "Airfoil",
    "name" : "A very flat plate with some thickness",
    "description" : "This member is not specified as part of AirfoilJSON but is valid because AirfoilJSON may have any number of members.",
    "coordinates" : [
        {"x" : 1.000000, "y" : 0.100000},
        {"x" : 0.000000, "y" : 0.100000},
        {"x" : 0.000000, "y" : -0.100000},
        {"x" : 1.000000, "y" : -0.100000}
    ],
    "uses" : "This plate may not good for containing food, but it is useful in explaining the basics of aerodynamics."
}
{% endhighlight %}

### Definitions ###

JavaScript Object Notation (JSON) and object, member, array, number, string are defined in [RFC 4627](http://www.ietf.org/rfc/rfc4627.txt).

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](http://www.ietf.org/rfc/rfc2119.txt).

The [UIUC airfoil data file site](http://www.ae.illinois.edu/m-selig/ads.html) contains data files in two similar but different text-based x/y coordinate file formats.  Selig refers to the format that contains a single list of coordiantes "starting from trailing edge, along the upper surface to the leading edge and back around the lower surface to trailing edge".  Lednicer is a similar format that contains two sets of coordinates with a blank line between them, "upper surface points leading edge to trailing edge" and "lower surface leading edge to trailing edge".  Lednicer also contains the number of coordinates for each surface as the second line of the file.  Both formats contain the name of the airfoil as the first line of the file.

### Status of this specification ###

This specification should be considered a **DRAFT** and subject to change based on usage and feedback.  If you have any questions or comments please [get in touch](mailto:mcroydon@gmail.com?subject=AirfoilJSON) or [fork this spec on GithHub and send a pull request](https://github.com/aeroneering/aeroneering.github.com).

## The AirfoilJSON Object ##

AirfoilJSON is always a single object.  This object represents an airfoil or a collection of airfoils.

* An AirfoilJSON object may have any number of members (name/value pairs).  AirfoilJSON does not impose any limits on members other than those mentioned in this specification.
* An AirfoilJSON object must have a member with the name "type".  This member's value must be a string, whose value must be either "Airfoil" or "AirfoilCollection".

### Airfoil Objects ###

* An Airfoil object is an AirfoilJSON object with a "type" member whose value is the string "Airfoil".
* An Airfoil object must have a member with the name "name".  The value must be a string and the string may be blank if a name for the airfoil is not known.
* An Airfoil object may have optional members "upper" and "lower" whose values are numbers indicating how many upper surface and lower surface points there are.  These values are often found in Lednicer format but not Selig (though they may be calculated for Selig).
* An Airfoil object must have a member with the name "coordinates".
  * The value of this member must be an array of objects.
  * Each object must have members with the names "x" and "y", each with a number value.
  * Each object represents an x,y coordinate for the airfoil.
  * Each object may have any number of members.

### AirfoilCollection Objects ###

* An AirfoilCollection object is an AirfoilJSON object with a "type" member whose value is the string "AirfoilCollection".
* An AirfoilCollection object must contain a member with the name "airfoils".  The value of this member must be an array of Airfoil objects.

## Appendices ##

### Appendix: Conversion from Lednicer to Selig ###

The normalization of AirfoilJSON to the Selig format is motivated by "Selig's x-y format is preferred" in the [UIUC airfoil contribbution section](http://www.ae.illinois.edu/m-selig/ads.html).  Note that some information is lost from a Lednicer format coordinate file.  Specifically the number of coordinates for the upper and lower surfaces as well as the grouping of the sets of points together.

### Appendix: Converting a UIUC Airfoil coordinate data file to AirfoilJSON ###

* The first line of the file becomes the "name" member after removing any leading or trailing whitespace.
* If the number of elements are listed on the second line, these are discarded or may be added as "upper" and "lower" members.
* Collect coordinates as a single list.
  * There is no additional work to do for Selig format.
  * For Lednicer format, process all points until the space.  This will be the top surface.  Reverse this list in order to start at the trailing edge like Selig.  Then append all points from the second list for bottom surface coordinates.
  * Treat numbers formatted such as (0.0019) as negative numbers like -0.0019.
  * Consider how to translate "......" number as found in naca4418.dat.
  * This list of coordinate objects, each with an x and y member becomes the coordinate member.

### Appendix: Considerations for future revisions ###

* Consider optional filename to tie back to UIUC database.  Or when needed just name filename.json instead of filename.dat.
* Consider using [JSON Hypertext Application Language](http://tools.ietf.org/id/draft-kelly-json-hal-05.txt) or a similar format for creating links back to UIUC or elsewhere, thereby promoting [HATEOAS](http://en.wikipedia.org/wiki/HATEOAS).
* Consider a version string or similar.  Current focus is replicating the information in the UIUC database but future versions could be oriented towards serializing equations and shapes/curves rather than points.
