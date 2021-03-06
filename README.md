MPAndroidChart
=======

A simple charting library for Android, supporting line- bar- and piecharts, scaling, dragging and selecting.

Remember: *It's all about the looks.*

For immediate access to our latest updates, please checkout the **experimental** branch. Beware that code on the experimental branch is not finished for release yet and might be incomplete or contain bugs.

Features
=======

**Core features:**
 - Scaling (with touch-gesture)
 - Dragging (with touch-gesture)
 - Highlighting values 
 - Save chart to SD-Card
 - Predefined color templates
 - Fully customizeable (paints, typefaces, legends, colors, background, gestures, ...)
 
**Chart types:**
 - **LineChart (single DataSet)**
![alt tag](https://raw.github.com/PhilJay/MPChart/master/screenshots/linechart.png)
 - **LineChart (multiple DataSets)**
![alt tag](https://raw.github.com/PhilJay/MPChart/experimental/screenshots/linechart_multiline.png)

 - **BarChart2D (single DataSet)**

![alt tag](https://raw.github.com/PhilJay/MPChart/master/screenshots/barchart2d.png)

 - **BarChart2D (multiple DataSets)**

![alt tag](https://raw.github.com/PhilJay/MPChart/master/screenshots/barchart2d_multi_dataset_date1.png)
![alt tag](https://raw.github.com/PhilJay/MPChart/master/screenshots/barchart2d_multi_dataset.png)

 - **BarChart3D**

![alt tag](https://raw.github.com/PhilJay/MPChart/master/screenshots/barchart3d.png)

 - **PieChart**

![alt tag](https://raw.github.com/PhilJay/MPChart/master/screenshots/piechart_selected.png)



Usage
=======

Rely on the **"MPChartExample"** folder check out the examples in that project. Furthermore, here is some code to get started.

**Setup:**

For using a <code>LineChart, BarChart or PieChart </code>, define it in .xml:
```xml
    <com.github.mikephil.charting.LineChart
        android:id="@+id/chart"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
``` 
```java
    LineChart chart = (LineChart) findViewById(R.id.chart);
``` 

or create it in code (and then add it to a layout):
```java
    LineChart chart = new LineChart(Context);
```   

**Styling:**

 - <code>setDescription(String desc)</code>: Set a description text that appears in the bottom right corner of the chart.
 - <code>setDescriptionTypeface(Typeface t)</code>: Sets the <code>Typeface</code> used for drawing the description text.
 - <code>setGraphOffsets(int left, int right, int top, int bottom)</code>: Sets the offsets of the graph in every direction, provide density pixels.
 - <code>setDrawYValues(boolean enabled)</code>: If set to true, the actual drawn values will be drawn next to the points, bars, or pie slices of the chart.
 - <code>setValuePaintColor(int color)</code>: Sets the color used for drawing the values if <code>setDrawYValues(...)</code> is enabled.
 - <code>setValueTypeface(Typeface t)</code>: Sets the <code>Typeface</code> used for drawing the values if <code>setDrawYValues(...)</code> is enabled.
 - <code>setValueDigits(int digits)</code>: Sets the number of digits to use for all printed values.
 - <code>setColorTemplate(ColorTemplate ct)</code>: Sets a <code>ColorTemplate</code> for the chart containing all colors. More information below.
 - <code>setPaint(Paint p, int which)</code>: Replaces the specified default <code>Paint</code> object with a new one. This method can be used to replace any predefined <code>Paint</code> object with an own <code>Paint</code> object and develop a completely personalized design.

**Getters and convenience:**
- <code>public ChartData getData()</code>: Returns the <code>ChartData</code> object the chart currently displays. It contains all information concerning the displayed values such as minimum and maximum values, value counts, value sums, ...
- <code>public float getScaleX()</code>: Returns the current scale factor on the x-axis.
- <code>public float getYChartMin()</code>: Returns the current minimum y-value that can be displayed by the chart - bottom line.
- <code>public float getYChartMax()</code>: Returns the current maximum y-value that can be displayed by the chart - top line.
- <code>public float getAverage()</code>: Returns the average value across all values the chart holds.
- <code>public float getAverage(int type)</code>: Returns the average value for a specific DataSet type in the chart. This type is the type provided in the <code>DataSet</code> constructor.
- <code>public PointF getCenter()</code>: Returns the center point of the chart in pixels.

 
**Gestures & Selecting:**

 - <code>setTouchEnabled(boolean enabled)</code>: If set to ture, touch gestures (such as scaling and dragging) will be possible on the chart. NOTE: If touch-gestures are disabled, highlighting on touch is disabled as well. Default: true.
 - <code>setOnChartValueSelectedListener(OnChartValueSelectedListener l)</code>: Sets a selection-listener to the chart that will generate callbacks when values are selected or unselected. The callback contains the selected values and their indices.
 - <code>setHighlightEnabled(boolean enabled)</code>: If set to true, highlighting/selecting values is possible on the chart. Default: true.
 
**Other:**
 - <code>saveToGallery(String title)</code>: Saves the current chart state as an image to the gallery.
 - <code>saveToPath(String title, String pathOnSD)</code>: Saves the current chart state as an image to the specified path.

**Adding data:**

If you want to add values (data) to the chart, it has to be done via the 

```java
    setData(ChartData data);
```
method. The <code>ChartData</code> class encapsulates all data and information that is needed for the chart during rendering. In the constructor, you can hand over an <code>ArrayList</code> of type <code>DataSet</code> as the values to display, and an additional <code>ArrayList</code> of <code>String</code> that will describe the legend on the x-axis.

```java
    public ChartData(ArrayList<String> xVals, ArrayList<DataSet> dataSets) { ... }
```

So, what is a <code>DataSet</code> and why do you need it? That is actually pretty simple. One <code>DataSet</code> object represents a group of entries (datatype <code>Entry</code>) inside the chart that belong together. It is designed to logically separate different groups of values in the chart. As an example, you might want to display the quarterly revenue of two different companies over one year. In that case, it would be recommended to create two different <code>DataSet</code> objects, each containing four values (one for each quarter). As an <code>ArrayList<String></code> to describe the legend on the x-axis, you would simply provide the four Strings "1.Q", "2.Q", "3.Q", "4.Q".

Of course, it is also possible to provide just one <code>DataSet</code> object containing all 8 values for the two companys. 

So how to setup a <code>DataSet</code> object?
```java
    public DataSet(ArrayList<Entry> yVals, int type) { ... }
```

When looking at the constructor, it is visible that the <code>DataSet</code> needs an <code>ArrayList</code> of type <code>Entry</code> and an integer value for the type. The type integer value can be chosen freely and can be used to identify the <code>DataSet</code> amongst other <code>DataSet</code> objects in the <code>ChartData</code> object. A possible type in this scenario could be a integer constant COMPANY_1.

The <code>ArrayList</code> of type <code>Entry</code> encapsulates all values of the chart. A <code>Entry</code> object is an additional wrapper around a value and holds the value itself, and it's position on the x-axis (the index inside the <code>ArrayList</code> of <code>String</code> of the <code>CharData</code> object the value is mapped to):
```java
    public Entry(float val, int xIndex) { ... }
```

Putting it all together (example of two companies with quarterly revenue over one year):

At first, create the lists of type <code>Entry</code> that will hold your values:

```java
    ArrayList<Entry> valsComp1 = new ArrayList<Entry>();
    ArrayList<Entry> valsComp2 = new ArrayList<Entry>();
```
Then, fill the lists with <code>Entry</code> objects. Make sure the entry objects contain the correct indices to the x-axis. (of course, a loop can be used here, in that case, the counter variable of the loop could be the index on the x-axis).

```java
    Entry c1e1 = new Entry(100.000f, 0); // 0 == quarter 1
    valsComp1.add(c1e1);
    Entry c1e2 = new Entry(50.000f, 1; // 1 == quarter 2 ...
    valsComp1.add(c1e2);
    // and so on ...
    
    Entry c2e1 = new Entry(120.000f, 0); // 0 == quarter 1
    valsComp2.add(c2e1);
    Entry c2e2 = new Entry(110.000f, 1; // 1 == quarter 2 ...
    valsComp2.add(c2e2);
    //...
```

Now that we have our lists of <code>Entry</code> objects, the <code>DataSet</code> objects can be created:
```java
    DataSet setComp1 = new DataSet(valsComp1, COMPANY_1); // COMPANY_1 is a constant integer and can be chosen freely
    DataSet setComp2 = new DataSet(valsComp2, COMPANY_2);
```
Last but not least, we create a list of <code>DataSets</code> and a list of x legend entries and build our <code>ChartData</code> object:

```java
    ArrayList<DataSet> dataSets = new ArrayList<DataSet>();
    dataSets.add(setComp1);
    dataSets.add(setComp2);
    
    ArrayList<String> xVals = new ArrayList<String>();
    xVals.add("1.Q"); xVals.add("2.Q"); xVals.add("3.Q"); xVals.add("4.Q"); 
    
    ChartData data = new ChartData(xVals, dataSets);
    chart.setData(data);
```

Now, our <code>ChartData</code> object can be set to the chart. But before drawing it, **colors need to be specified**. If no colors are specified when the data is drawn, the default <code>ColorTemplate</code> will be used.


**Setting colors:**

Setting colors can be done via the <code>ColorTemplate</code> class that already comes with some predefined colors (constants of the template e.g. <code>ColorTemplate.LIBERTY_COLORS</code>). 

Explaination: The <code>ColorTemplate</code> basically has two methods for setting colors:

 - <code>addDataSetColors(int[] colors, Context c)</code>: This method will add a new array of colors for the <code>DataSet</code> at the current index. The current index starts at 0 and depends counts up per call of this method. If no calls of this method have been done before, the colors set in this call will be used for the <code>DataSet</code> at index 0 in the <code>ChartData</code> object. Upon calling this method again on the same <code>ColorTemplate</code> object, the provided color values will be used for the <code>DataSet</code> at index 1.
 
 - <code>addColorsForDataSets(int[] colors, Context c)</code>: This method will spread the provided color values over an equal amount of <code>DataSet</code> objects, using only one color per <code>DataSet</code>.

In our example case, we want one color for each <code>DataSet</code> (red and green), which will mean, that all entries belonging to the same <code>DataSet</code> will have the same color:
```java
    ColorTemplate ct = new ColorTemplate();
    ct.addColorsForDataSets(new int[] { R.color.red, R.color.green }, this);
    chart.setColorTemplate(ct);
```

It would also be possible to let each <code>DataSet</code> have variations of a specific color. For example company 1 should have 4 colors from light to dark red, and company 2 should have 4 colors from light to dark green. In that case, we specify a color array for each <code>DataSet</code>:
```java
    ColorTemplate ct = new ColorTemplate();
    ct.addDataSetColors(redColors, this); // redColors is an array containing 4 colors
    ct.addDataSetColors(greenColors, this);
    chart.setColorTemplate(ct);
```

More documentation and example code coming soon.



License
=======
Copyright [2014] [Philipp Jahoda]

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

**Special thanks** to [mikegr](https://github.com/mikegr) and [ph1lb4](https://github.com/ph1lb4) for their contributions to this project.
