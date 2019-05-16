# Test MD formatting from Bitbucket

# WILL 3.0 - Data format specification

## Abstract
This document describes the WILL data format. The WILL data format serves as a data model for digital ink, a data encoding scheme that supports pen properties and contextual (domain) meta data.  

# Introduction
Taking down a phone message, sketching ideas on a whiteboard in a meeting, completing a form at a counter, or signing a mortgage application is part of our daily lives. Writing has a very long history. It began as simple pictographs drawn on rock, then combined to represent ideas and developed into more abstract symbols. Just like our writing today, early symbols were used to store information and communicate it to others.

In recent years, modern technology has dramatically changed the way we communicate through writing. However, despite the increased use of computers for writing, the skill of handwriting remains important in education, employment and in everyday life. As Wacom is one of the drivers of adoption of handwriting on mobile devices and portable computers, the Wacom Ink Layer Language (WILL) is based on years of experience in ink devices and digital ink.

This document describes the WILL 3.0 data format. The format serves as a data model with additional metadata for digital ink representing ink entered with an electronic pen or stylus. Digital ink defined in the WILL data format can be serialised into a binary stream by several serialisation schemes and stored in files in various formats such as JSON or Google protobuf binary format. 

## Uses of WILL 3.0
WILL 3.0 is a universal ink standard developed by Wacom to be used in a number of applications such as form filling,  biometric signature workflows, note-taking, document annotation, ink collaboration, inking in extended reality (XR) environments or creative sketching applications. Its purpose is to expand the areas where pen and ink can be used as a convenient and natural form of input. In the following are examples of its use:

### Ink Real-Time communication
Shared ink canvas offers mobile-device users a compelling new way to communicate. Users can draw or write with a pen on the device's screen to compose notes and sketches in their own handwriting. 

### Electronic Form-Filling 
A wide range of processes in many industries, including healthcare, financial services and government are still dependent on the handwritten completion of forms. So, in the real world, digitising business processes is more about integrating digital and paper documents within workflows, than it is about replacing one with the other. 

### Pen Input and Multimodal Systems
Applications utilising Digital Ink and Multimodal Input with the capacity to record, analyse and present a broad range of data in real time – from timestamps to geolocation, emotional and handwritten data – offer a vast potential across a broad range of industry sectors, from education to psychology and medicine.


### Note-Taking
Making notes, sketching out a process or doodling around a concept – these are still the most natural ways for us to record, create and share information and ideas. And every pen stroke is uniquely personal. In the past, there hasn’t been a way to ensure this personality is retained when using digital ink with any existing or new application and on every platform. There is now.


### Education
It’s a fact: Cognitive psychology and neuroscience have revealed that handwriting promotes creativity, deeper understanding and better information recall. Handwriting contributes to the formative learning process of reading, writing and spelling, the comprehension of meaning, memory, problem solving, and imagination in artistic and creative tasks **[James 12, James 17]**. This is because handwriting is a complex cognitive process involving neuro-sensory experiences and fine motor skills: feel the pen and paper, hold the pen, and direct the movement of the pen by thought. 


### Signature Verification
Signing documents in person represents a cultural norm that indicates an emotional, personal commitment on the part of the person signing. However, there is a glaring mismatch between the on-demand expectations of our always-on world, and the inefficient reality of sending signed paper documents back and forth through the post.

### Inking in Extended Reality
Modern mobile devices are equipped with a good camera and have sufficient computational performance to support augmented reality (AR). 
Moreover, AR engines such as Vuforia are available as well as build-in engines like ARKit on iOS, and ARCore on Android/iOS offering a base technology for AR. 
There are new devices on the market for virtual reality (VR) such as HTC Vive and Oculus Rift, and new devices for mixed reality (MR) such as HoloLense and Magic Leap One. 
All of these environments lack a precise specification for the input of data. 
While being in an immersive environment ink can be also used in a review scenario or a brain storming session on virtual whiteboards. 

## Scope
This document defines a data model for digital ink, referred to as the WILL 3.0 data format. 


## Terms and definitions
For the purposes of this document, the following terms and definitions apply. 

**Ink Device**

An *Ink Device* is a computer input device that enables a user to hand-draw images, animations and graphics, with a special pen-like stylus, similar to the way a person draws images with a pencil and paper.

**Sensor Data**

RAW data comprises the signals measured by the sensor technology used to capture the user interaction with the *Ink Device*. 
Firmware and driver software process the RAW data and provide the *Sensor Data* via a predefined API of the operating system.

**Path**

Visual entity used for the visual representation of a path, trace, or trajectory of the electronic pen.  Each instance of Path includes one or more Segments.  

**Position**

Property of the Segment.  Position includes two dimensional data (X, Y) which are used as a control point for a Spline curve algorithm (Catmull-Rom curve).

**Segment**

Entity used to represent a part (segment) of the Path. Each instance of Segment includes Positions.

**Digital Ink**

*Sensor data* produced by an *Ink Device* such as electronic pen or stylus which is transformed using computer graphic algorithms to a visual representation. *Digital Ink* can include several Paths or a volume in 3D space.

**WILL Data Format**

The data model describing WILL digital ink. 


# Concept
WILL 3.0 focuses on a broader acceptance in the digital ink market and it is design to support various use cases via a universal data format. 

In the following its major design goals are introduced:

## Interoperability
WILL 3.0 is platform and device agnostic, thus the specification does not focus on specific hardware platforms. The format contains a section to define the characteristics of the Ink Device. Sensor information which is available for all digitiser devices (see Figure 1) is:

* **X, Y** – coordinates on a surface 
* **Timestamp** – when the sample was produced 

Dependent on sensor technology:

* **Z** - Position in third dimension (tracking of the Ink Device in three-dimensional space)
* **Pressure** – measuring force applied on the surface 
* **Pen orientation** – pen tilt as represented by angles measured from two axes 
* **Pen rotation** – rotation sensitivity captured by the sensor to create different effects 
* **Pen events** - events triggered by buttons, or other sensors attached to sensor 

There are existing formats used for storing digital ink and these are listed in the Normative References section. 
Ink Markup Language **[InkML]** and Ink Serialized Format **[ISF]** are the most well-known formats for storing digital ink besides Wacom Ink Layer Language **[WILL]**. 
The previous version of WILL did not support the storage of sensor data samples for digital ink, e.g., timestamps, (x,y)-coordinates, or pressure values.  Given the ability to store ink sensor samples along with the visual representation of digital ink as well as semantic metadata, a loss-less conversion of the WILL data format to existing formats is now possible.

![Overview ink sensor channels.](./media/01_overview_ink_device_sensor_channels.png)
*Figure 1: Overview ink sensor channels.*


## Natural

Digital ink has to look natural and similar to real ink. To ensure that the visual appearance of the digital ink can be shared across platforms, WILL 3.0 contains all relevant data to present ink in a consistent way across all platforms. 

## Active

Mobile apps, Cloud application, or Cloud services have become a part of modern IT infrastructures. Thus, modern data formats need to address issues including:

* Unique identifiable IDs for devices 
* Streaming capability for partial updates of ink data
* Document size reduction
* Support for commonly used Web Standards such as JSON

## Semantics
There are three types of metadata: descriptive, structural and administrative. Descriptive metadata are typically used for discovery and identification, as information used to search and locate an object such as title, author, subject, keyword and publisher. WILL offers metadata for the Section Document and Author in order to describe the ink document. Structural metadata give a description of how the components of an object are organised. The metadata for ink can be used to describe the structure of ink documents. Finally, administrative metadata give information to help manage the source. They refer to the technical information including file type or when and how the file was created. Two sub-types of administrative metadata are rights management metadata and preservation metadata. Rights management metadata explain intellectual property rights, while preservation metadata contain information that is needed to preserve and save a resource. This type of metadata is included in the Section Document.

Semantic metadata is slowly becoming the differentiator in the vocabulary of many vendors. Semantics is the study of meaning. As it applies to technology and unstructured content, it represents the ability to extract meaning from words, phrases, sentences, and larger units of text that provide the context within content, and is manually applied or automatically generated as semantic metadata to describe the object. Semantic metadata is typically leveraged to improve search, but any application that uses metadata can achieve significant benefits through the generation or application of semantic metadata. Thus, WILL metadata is based on established metadata for document and author descriptions and defines its own semantic metadata for ink. 

## Biometrics
Another important objective in the design of the WILL is to support the capture of biometric data from ink devices. Biometric data is used in the context of signature capture and verification. Some ink devices provide additional information that in most use cases is considered less important and may not be supported by all makes or types of devices.

This includes:

* **Pressure** - the force applied to the nib of the pen
* **Inclination** - the angle between the pen barrel and vertical
* **Orientation** - the plain-direction of the pen from the nib
* **Rotation** - the rotation of the barrel during signing

The forensic character of the data is of paramount importance, and means that the data-collection philosophy differs in many respects from competing signature technologies. A key principle is that during the collection of the signature the software stores the data exactly as it is supplied by the device. Each type of data (i.e. position, time, pressure etc) is collected with metric information which describes the units being used by the device, and this is stored with the raw point data to allow the conversion to true units when required. The advantage of this is that the accuracy of the information is determined by the device and cannot be compromised by the conversion process. In addition to the pen-data, contextual data is collected and stored with the signature. This includes:

* The name of the signatory
* The date and time at which the signature was given
* The reason for signing
* The make and type of digitizer being used
* The type and version of the digitizer driver being used
* The type and version of the operating system of the client PC being used
* The Network Interface Card address of the PC

The objective is to store sensor data and the characteristics of the ink device alongside the visual representation of the digital ink.

# Data Model
The WILL data model describes WILL digital ink.

## Sensor Input
One of the design requirements for the WILL 3.0 format is to allow accurate recording of metadata about the format and quality of ink as it is reported by the ink device. The Ink Device is typically a digitizer device such as graphics tablets, smart pen, pen displays, stylus, etc. This is accomplished in the Ink Device entity, which records basic information about the manufacturer and model of the device and sensor channels, with detailed information about channel characteristics defined in InkSensorChannel. Each InkSensorChannel has a specific InkSensorType with specific characteristics including:

* An assigned name
* Unit of the tracked data
* A min and max value, which can be used for normalisation
* Resolution
* Sampling rate if it differs from the device specific one
* The data type (DataType) of the RAW data
* A flag if the sampling is variable or constant

### InkSensorMetricType
Each sensor channel has an associated metric. For each metric the intention is to leverage the International System of Units (SI).

|Type         | SI unit   | Description               |
| :---        |    :----:   |          :---           |
|LENGTH        |	metres      |Length metric, used for coordinates.  |
|TIME          |seconds      |Time metric.       |
|FORCE         |Newton       |Force metric.
|ANGLE         |radians      |Angle.              |
|NORMALIZED    |             |Percentage, expressed as a fraction (1.0 = 100%). |
|LOGICAL_VALUE |             |Logical boolean value. |

### DataType
To encode sensor data collected from the different sensor channels the data needs to be encoded with a data type:

|Type         | Description                            | 
| :---        |    :----                               |
|BOOLEAN	    | Boolean datatype                       |
|FLOAT32	    | 	Floating point datatype (32 Bit).     |
|FLOAT64	    | 	Floating point datatype (64 Bit).     |
|INT32	       | 	Integer datatype (32 Bit).            |
|INT64	       | 	Integer datatype (64 Bit).            |
|UINT32	    |  Unsigned integer datatype (32 Bit).   |
|UINT64	    | 	Unsigned integer datatype (64 Bit).   |


### EncodingMethod
The encoding of the sensor data is either via direct encoding of the values as bytes or via delta encoding.

|Type         | Description                                             | 
| :---        |    :----                                                |
|DIRECT       | Values are directly stored according to their DataType. |
|DELTA        | Differences in directly-encoded values are stored.      |

### InkSensorType
The InkSensorType enumeration defines a set of known sensor channels available for ink devices (see Figure 1).

|Type         | Metric          | Description |
| :---        |    :----                               |:---- |:---- |
|CUSTOM	    |         |Custom channel. This can be used if none of the existing channels matches the sensor type.|
|X	           | LENGHT  |X coordinate. X-position in the 2D / 3D.                                                  |
|Y	           | LENGHT  |Y coordinate. Y-position in the 2D / 3D.                                                  |
|Z	           | LENGHT  |Z coordinate. Z-position in the 3D.                                                       |
|PRESSURE	    | FORCE   |Force applied to the ink device.                                                          |
|TIMESTAMP  	 | TIME  	 |Time stamp delta for the channels.                                                        |
|RADIUS_X     | LENGTH  |Touch radius by X.                                                                        | 
|RADIUS_Y     | LENGTH  |Touch radius by Y.                                                                        | 
|AZIMUTH	    | ANGLE   |Azimuth angle of the pen (yaw).                                                           |
|ALTITUDE   	|  ANGLE   |Elevation angle of the pen (pitch).                                                       |
|ROTATION	    | ANGLE   |Rotation (counter-clockwise rotation about pen axis).                                     |

To represent the tilt angle of an Ink Device Figure 2 uses azimuth *(OA)* and elevation angle *(OE)*. 

![Representation of tilt angle with azimuth and elevation angle.](./media/02_03_detailed_visuals_channels_TEXT.png)
*Figure 2: Representation of tilt angle with azimuth and elevation angle.*

Figure 3 shows the rotation angle (OR).
![Rotation of the pen about the pen axis.](./media/02_02_detailed_visuals_channels_TEXT.png)
*Figure 3: Rotation of the pen about the pen axis.*

### InkSensorChannelGroup
Depending on the ink device there are different sensor data channels available. Some channels are grouped as they depend on the same sensor technology, thus are sampled at the same rate.


**ATTRIBUTES**

|Attribute         | Data type            | Required | Default| Description|
| :---             | :----                |:----     |:----   |:----                 |
|id	                | string               |Yes       |        |ID of sensor channel group. |
|channels	         | InkSensorChannel[]   |Yes       |        |Group of channels which logically belong together and the same timestamp per sample point.|
|samplingRateHint  | uint32	            |No        |0        |Hint for the intended sampling rate of the channel .|
|latency           | uint32               |No        |0        |Latency measure in milliseconds, introduced by hardware, firmware, and operating system.. 

### InkSensorChannel
The InkSensorChannel describes information for a specific sensor channel. These specification details for the channels are relevant for normalisation of data if it is used to within machine learning.

**ATTRIBUTES**

|Attribute       | Data type            | Required | Default| Description|
| :---           | :----                |:----     |:----   |:----                 |
|id	              | string               |Yes       |        |ID of sensor channel. |
|type            | InkSensorType        |Yes       |        |Type of the sensor.|
|name	          | string               |Yes       |        |Name of the channel. Predefined channels are represented in the sensor channel.|
|metric          |	InkSensorMetricType	|Yes       |        |Indicates metric used in calculating the resolution for the data item.|
|min             |	float	             | No       | 0.f    |Minimal value of the channel. For normalisation of channel values.    |
|max             | float	       | No | 0.f | Maximal value of the channel. For normalisation of channel values.|
|resolution      |	float	|Yes	|0	|Is a decimal number giving the number of data item increments per physical unit., e.g. if the physical unit is in m and device metric resolution is 100000, then the value 150 would be 0.0015 m.|

### InkDevice 
The InkDevice provides all relevant information about the digitiser used for capturing the ink.

**ATTRIBUTES**

|Attribute         | Data type                      | Required | Default| Description|
| :---             | :----                          |:---- |:---- |:---- |
|id	                | string	 | Yes |	| Unique ID within the WILL data.|
|manufacturer       |string   | No	| |Manufacturer of the ink device or the device the ink device is attached to.|
|model              |string	|No	|| Model name of the ink device or the device the ink device is attached to.|
|serialNumber       |string	|No	|| Serial number for the device.|
|manufacturerID    |string    |No	| |	Globally unique identifier for ink device (e.g., Wacom Pen ID, MAC-Address, serial number, ...). If the ink device does not have an identifier on its own, the device which the ink device is attached to must provide the identifier.|
|channelGroups|InkSensorChannelGroup[]	|Yes||Sensor channels defined for Ink device.|

**Example**

Depending on the use case the information stored as RAW data can be defined in the appropriate way.

For instance for a Bamboo Smart pad device the configuration can be like this. Only x- and y-coordinates can be stored in meters.

```json
{
	"devices": {
	    "data": [
	        {
	            "id": "d0",
	            "manufacturer": "Wacom Co., Ltd.",
	            "model": "Bamboo Smartpad",
	            "channelGroups": 
	            [
                    {
                        "channels": [  
					                {
					                    "id": "0",
					                    "type": "X",
					                    "name": "X Channel",
					                    "metric": "LENGTH",
					                    "resolution": 100000,
					                    "min": 0.0,
					                    "max": 21600.0
					                },
					                {
					                    "id": "1",
					                    "type": "Y",
					                    "name": "Y Channel", 
					                    "metric": "LENGTH",
					                    "resolution": 100000,
					                    "min": 0.0,
					                    "max": 29700.0
					                },
					                {
					                    "id": "2",
					                    "type": "PRESSURE",
					                    "name": "Pen pressure",
					                    "unit": "FORCE",
					                    "resolution": 2048,
					                    "min": 0.0,
					                    "max": 2047.0f
					                },
					                {
		                               "id": "3",
		                               "metric": "TIME",
		                               "resolution": 1000,
		                               "type": "TIMESTAMP"
                                }
					              ],
					       "id": "XY",
					       "samplingRateHint": 200 
					   }
	            ]
	        }
	    ]
	}
}
```

## Ink
The basic element of digital ink is the ink stroke. WILL distinguishes between sensor data sequences captured by an ink device (digitizer) and its visual representation the Path; the digital ink. The sensor data will be represented in *SensorData* storing the sequence of data samples recorded for each data channel whereas *Path* stores the visual appearance of the stroke itself. The *Path* itself is dependent on the path generation algorithm and its parameters. For instance, the pen orientation will result in a tilted ink path, or the pressure channel will result in different thickness of the ink path. Thus, the results of the path generation will be handled in a different structure. 

### Sensor Data
This section formalizes the sensor data from a pen device. For the pen devices, several basic notations for the online pen data have to be introduced. Mainly defining a time ordered sequence of (x; y) coordinates and further pen sensor data, identifying the path of a pen while writing on a surface, e.g., paper, display. 

Every digital ink-enabled pen technology - stylus, smart pen, smart pad - provides similar pen or touch input data. Technologies mainly differ in their sampling rate, resolution, and if they provide additional sensor data, such as pressure, pen rotation, or pen inclination (azimuth and elevation angles). Sample data is taken at a fixed interval:

Available for all pens:

* **X, Y** – coordinates on a surface
* **Timestamp** – when the sample was produced

Dependent on sensor technology:

* **Pressure / force** – measuring force applied on the surface
* **Pen orientation** – pen tilt as represented by angles measured from two axes. Pen angle along x-axis / azimuth (0 - 180°) and pen angle along y-axis / elevation (0 - 180°)
* **Pen rotation** – rotation sensitivity (0 - 360°) captured by the sensor to create different effects


### InkState
The *InkState* enumeration defines the state of ink recorded.

Depending on the digitizer technology the position of the ink device while it is not in contact with a surface can be reported. This is the hovering state and can be recorded in the WILL 3.0 data format.

Hover data can be used in healthcare scenarios such as hand tremor detection, indicating Parkinson’s disease. Hover data can also be used in signature verification as additional biometric data stored with the visible part of a signature.

For virtual, augmented, and mixed reality applications the concept of digital ink in three-dimensional space is supported. While tracking ink in a three-dimensional space, there is a need to define states where the device is producing ink and where the device is only hovering. 

WILL 3.0 supports different modes:

|Type             | Description                            | 
| :---            |    :----                               |
| PLANE	        | Ink device is moving on a surface.     |
| HOVERING	     |Ink device is hovering over a surface.  |
| IN_VOLUME	     | Ink device is moving in the air, with active inking.|
| VOLUME_HOVERING |	Ink device is hovering in the air, without active inking.|
| START_TRACKING	 | For Hovering and VR entering the proximity sensor needs to be flagged, as well ... |
| STOP_TRACKING	 |as leaving the proximity sensor respectively VR tracking.|

### InkDeviceContext
*InkDeviceContext* defines the device id used to record the ink within a unique context which can be referenced within the meta data part of the format. 

**ATTRIBUTES**

|Attribute         | Data type         | Required  |  Description                                      |
| :---             |    :----          |:----      | :----                                             |
|id                |string	            |Yes         |Unique identifier for capturing context.          |
|inkDeviceID       |string.            |Yes        |References to InkDevice.                           |
|offset            |Vector3            |Yes        |Sets the origin of the relative coordinate system. | 

### SensorData
SensorData is the base structure used to store sensor data sequences. Each sequence has a unique id to reference them within the grouping, the associated context, the creating timestamp, the data channels depending on the configuration described within the ink device structure, and its state.

**ATTRIBUTES**

|Attribute         | Data type         | Required |  Description|
| :---             |    :----          |:---- | :---- |
|id	|string  |	Yes	 |Unique identifier for each stroke.|
|inkDeviceContextID| string | Yes	| References a context ID.|
|timestamp |int64|Yes|	Timestamp for first sample of the stroke.|
|dataChannels|DataList[]	|Yes|	Data channels encoding the data list. |
|state | InkState	| Yes	| State of the ink.|

### DataList
DataList is the base structure used to store data. As the list is generic and can be used to store different data types the data type to encode the value is bytes. In order to achieve a good compression differential integer data encoding is applied. 

**ATTRIBUTES**

|Attribute         | Data type    | Required   |  Description                                                 |
| :---             |    :----     |:----       | :----                                                        |
|type	            | DataType     |Yes         | Type of the data list.                                       |
|data	             | bytes     	 |Yes        | Data encoded using differential integer data encoding.       |
|channelID	      |string	        |Yes        | Referencing InkSensorChannel via id.                         |
|precision         |uint32         |Yes       |Precision of integer encoding, needed for encoded float values.|
|compressionMethod |string         |Yes       |Algorithm used to compress data sequence.                      |

#### Encoding 
The encoding process works as follows:

**Step 1. (optional)** Convert floating-point values to integer values using externally-specified decimal precision:

```
integerValue = floatValue * (10 ^ decimalPrecision);
```
***Note:*** Step 1 is only applied if there the datatype is not an integer datatype (e.g., UINT32, UINT64, INT32, or INT64). 


**Step 2.** Perform delta encoding on the converted values:

```
encodedValues[0] = integerValues[0];
for(i = 1; i < n; i++)
{
    encodedValues[i] = integerValues[i] - integerValues[i - 1];
}
```

***Note:*** In converting between floating-point values and fixed-point (integer) values, some precision can be lost. If precision is critical to application functionality, you must take this into consideration. A possible solution is to rely on IEEE 754: in this case, for every value to be converted, you should use the appropriate decimalPrecision and an approximation that remains unchanged during this conversion.

#### Decoding 
To restore the original values, the encoding process is reversed, as follows:

**Step 1.** Perform delta decoding to restore the integer values:

```
integerValues[0] = encodedValues[0];
for(i = 1; i < n; i++)
{
    integerValues[i] = integerValues[i - 1] + encodedValues[i];
}
```

**Step 2. (optional)** Convert integer values to floating-point values using externally-specified decimal precision:

```
floatValue = integerValue / (10 ^ decimalPrecision)
```

***Note:*** Step 2 is only applied if there the datatype is not an integer datatype. 

## Visual Appearance
The visual appearance is defined by the geometry algorithm and its parameters, based on the input from the digitiser. The sensor data has an impact on the capabilities of the rendering, for instance if the orientation of the ink device can result in a tilt effect which is shown in Figure 5. 

![Tilt effect caused on a tilted ink device.](./media/08_tilt.png)

*Figure 5: Tilt effect caused on a tilted ink device.*

WILL 3.0 rendering is split into two major parts (see Figure 6): the computation of the *Ink Geometry* which results in 2D rendering primitives such as polygons, and the rasterization part. 
Raterization takes ink paths described in 2D rendering primitives and converts them into a raster image. 
The rasterized image may then be displayed on a computer display, a VR headset, or stored in a file format (e.g., PDF, PNG).

The *Ink Geometry Pipeline* consists of a chain of processing blocks *(processors, producers, etc.)*. 
The input of the pipeline is *sensor data sequence*, which passes through a set of processing stages. 
The output of each stage is taken as input by its successor. 
The main goal of the pipeline is to create digital ink. However, its generic implementation allows a much broader set of capabilities.

Vector rendering can be done by using each platform's specific 2D rendering APIs. In most cases they use Bezier curves as path objects, which can be created easily by connecting the vertices of the polygons produced by WILL 3.0 pipeline. The result of this is a filled Bezier contour, representing the path. For raster rendering OpenGL shaders are used.

![Rendering pipeline for WILL 3](./media/03_pipeline.png)

*Figure 6: Rendering pipeline for WILL 3.*

The processing blocks of the pipeline (see Figure 7) are explained in the following:

1. **Path producer**: Converts the input into internal path representation based on the supplied layout and the path is smoothed via Double Exponential Smoothing.
2. **Spline producer**: Adds two control points, one in the beginning and one at the end of the path in order to produce Catmull-Rom spline.
3. **Spline interpolator**: Defines the spline by adding points along its trajectory, spaced out according to the spacing parameters.
4. **Brush applier**: Converts the spline into a collection of point sets, by replacing the points with brush polygons.
5. **Convex Hull chain producer**: Creates a convex hull around each two consecutive point sets, using the monotone chain algorithm.
6. **Polygon builder**: Merges polygons using Union. Takes the curve composed of line segments (polygons) and finds a similar curve with fewer points, using the Ramer–Douglas–Peucker algorithm **[ Douglas 73]**.

![Steps within the Ink Geometry pipeline.](./media/04_rendering.png)

*Figure 7: Steps within the Ink Geometry pipeline.*

The following sections show how the digital ink is stored.

### CompositeOperation

|Type             | Description                            | 
| :---            |    :----                               |
|COPY|The destination is overwritten with the source|
|SOURCE_OVER|Source is blended on top of destination|
|DESTINATION_OVER|Source goes behind the destination|
|DESTINATION_OUT|The destination is kept where it doesn't overlap the source|
|LIGHTER|Color values of source and destination are added|
|DIRECT_MULTIPLY|Color values of source and destination are multiplied|
|DIRECT_INVERT_SOURCE_MULTIPLY|For each color component source is multiplied by destination and the product is subtracted from 1|
|DIRECT_DARKEN|Retains the darkest pixels of both layers|
|DIRECT_LIGHTEN|Retains the lightest pixels of both layers|
|DIRECT_SUBSTRACT|Subtract destination color components from source color components|
|DIRECT_REVERSE_SUBSTRACT|Subtract source color components from destination color components|


### RotationMode
|Type             | Description                            |
| :---            |    :----                               |
|NONE|The particles are not rotated|
|RANDOM|The particles are rotated randomly|
|TRAJECTORY|The particles are rotated to follow the stroke's trajectory|


### VectorBrush
A brush defined with a convex polygon (convex polyhedron for 3D).

**ATTRIBUTES**

|Attribute         | Data type         | Required |  Description|
| :---             |    :----          |:----     | :---- |
|id	|string|Yes||
|pointX|	float[]|	Yes	|A list containing the X coordinates of the brush vertices|
|pointY|	float[]|	Yes	|A list containing the Y coordinates of the brush vertices|
|pointZ|	float[]|	No	|A list containing the Z coordinates of the brush vertices|
|indices|	uint32 |3D Only |A list of vertex indices that denote the polyhedron's triangles|
|compositeOperation|CompositeOperation|Yes|Composite operation used for 2D brushes|

### RasterBrush
A brush defined with raster images.

**ATTRIBUTES**

|Attribute         | Data type         | Required |  Description|
| :---             |    :----          |:----     | :---- |
|id	|string|	Yes|Brush identifier|
|shapeTexture | bytes[]|Yes|A png image that contains the shape texture|
|spacing|float||Distance between neighbour particles|
|scattering|float||The scattering along the curve normal|
|rotation|RotationMode||The particle rotation mode of the brush|
|fillTexture|bytes[]|Yes|A png image that contains the fill texture|
|fillWidth|float||Width of the fill tile|
|fillHeight|float||Height of the fill tile|
|randomizeFill|bool||Specifies whether the fill texture is randomly displaced|
|compositeOperation|CompositeOperation||Composite operation used for 2D brushes|


### Brushes
A collection of the existing vector and raster brushes.

**ATTRIBUTES**

|Attribute         | Data type         | Required |  Description|
| :---             |    :----          |:----     | :---- |
|vectored	|VectorBrush|No||
|rastered |RasterBrush|No||

### Style
Visual settings of processed path.

**ATTRIBUTES**

|Attribute         | Data type         | Required |  Description|
| :---             |    :----          |:----     | :---- |
|width	|float|Yes|Static width of stroke when dynamic is not needed|
|color |fixed32|Yes|Stroke color|	
|brushID|string|Yes|Reference to Brush used for stroke rasterization|	
|particlesRandomSeed|uint32|Yes|Particles brush seed|
|compositeOperation|CompositeOperation|Yes|Rasterization composite operation|


### Path
Digital ink defined by InkML **[InkML]**, SVG **[SVG]**, and WILL Data Format **[WILL]** primarily represents the path (or trace/trajectory) of the ink device. In the WILL Data Format, data representing a pen path is referred to as a Path. A *Path* corresponds to the ‘path’ element in SVG and the <trace> element in InkML. In other data format specifications a Path may be referred to as a ‘stroke’ or ‘stroke data’.

The Path entity is the core building block in digital ink.  Every stroke or trace is represented by an instance of Path. 

Path in its simplest form contains one or more Segments (Catmull-Rom), which include a series of positional points, i.e. Positions.  But, to achieve a high quality digital ink experience, Path includes Stroke Width and Stroke Color as its properties.   In addition to those properties, digital ink in WILL Data Format includes Start Parameter and End Parameter as Path entity properties. 

**ATTRIBUTES**

|Attribute         | Data type         | Required |  Description|
| :---             |    :----          |:----     | :---- |
|id	|string|Yes|Identifier of path|
|splineX|float[]|Yes|Spline values for x|
|splineY|float[]|Yes|Spline values for y|
|splineZ|float[]|No| Spline values for z|
|size|float[]	||Size values|
|rotation|float[]||Brush rotation.|
|scaleX|float[]||Brush scale x|
|scaleY|float[]||Brush scale y|
|scaleZ|float[]||Brush scale z|
|offsetX|float[]||Brush offset x|
|offsetY|float[]||Brush offset y|
|offsetZ|float[]||Brush offset z|
|color|uint32[]|Yes|Color coding|
|startParameter|float|Yes|This parameter specifies the t-value ( [ 0, 1 ] ) of the first instance of Segment (Catmull-Rom) of multiple instances of Segments at which display or rendering of the instance of Path starts.  Default value is 0.  Please note that this Start Parameter exists only in the first of the instances of Segments in a Path.| 
|endParameter|float|Yes	|This parameter specifies the t-value ( [ 0, 1 ]) of the last instance of Segment of multiple instances of Segments at which display or rendering of the instance of Path starts.  Default value is 1.  Please note that Start Parameter exists only in the last of the instances of Segments in a Path.|

The geometry (i.e. shape of curve) of each instance of a Segment is reproduced by Catmull-Rom splines with four Positions. Position includes two-dimensional data (X , Y), which is used as control points for the Catmull-Rom spline curve algorithm.   Geometries (shapes) of multiple Segments based on Catmull-Rom spline are represented with a series of the two-dimensional data, Positions (P0, P1, P2, ... Pn). The geometry of the first instance of Segment is defined by four Positions(P0, P1, P2, P3). Figure 8 illustrates the geometry of the i-th instance of Segment.  

The geometry of the i-th instance of Segment is determined by four Positions (Pi-1, Pi, Pi+1, Pi+2 ). The tangent at each Position is calculated using the previous and next Position on the spline.


![Geometry of the i-th instance of Segment](./media/09-index.png)

*Figure 8: Geometry of the i-th instance of Segment*

Delta encoding/decoding is applied to all values of Position within a Path, as well as the corresponding value(s) of Stroke Width. 

## Grouping
The core structure used to assign semantic statements to ink is the grouping mechanism. 
In most cases a single ink stroke does not contain meaningful information, only a collection or group of strokes does, 
for instance as illustrated in the Figure 9, where stroke s8, s9, s10, and s11 form the word "digital". 
The groups are handled in industry standard tree data structures holding a collection of nodes for ink strokes and related sensor data. 
For example, in Figure 10 g4 is the word "hello", g5 "world", and g6 the punctuation symbol "!". 
These are combined in g2 to represent a text line which further is part of a paragraph. 
A similar structuring mechanism is used in Rich Text editing and here it is applied to Ink Documents.


![Example ink document with groups](./media/05_grouping.png)

*Figure 9: Example ink document with groups*

![Hierarchy of groups](./media/07_group_tree.png)

*Figure 10: Hierarchy of groups*

An ink group combines a list of elements, either:

* Sensor data sequences,
* Ink Paths,
* Chunks of RAW sensor data,
* Chunks of ink path, 
* External elements.

### GroupElementType
The GroupElementType defines the type of the item in the relationship model.

|Type             | Description                            | 
| :---            |    :----                               |
|GROUP            |Group element                           |
|PATH	           |Visual path element                     |
|SENSOR_DATA	    |Sensor data element                      |
|CHUNK	           |Partial element                         |
|EXTERNAL	       |External reference                       |

### ChunkType
The ChunkType defines which type of chunk is described. 

|Type              | Description                            | 
| :---             |    :----                               |
|PATH_CHUNK        |Path chunk                              |
|SENSOR_DATA_CHUNK |	Sensor data chunk

### Groups
Groups structure contains all groups and their relationship.

**ATTRIBUTES**

|Attribute         | Data type         | Required |  Description|
| :---             |    :----          |:----     | :---- |
|data              |Group[]            |Yes |Contains a list of groups available.
|relations         |GroupItem[]        |Yes |Contains a list of group items defining the relationship among the items.|
|chunks            |Chunk	             |No |Contains group elements which define a partial element.|
|externalItems.    |ExternalItem       |No  |Contains a list of external elements in the container document.|

### Group
Group is the base structure for grouping ink data.

**ATTRIBUTES**

|Attribute         | Data type         | Required |  Description|
| :---             |    :----          |:----     | :---- |
|id                |string             |Yes       |Unique identifier for each group.|
|bounds            |Rectangle          |No        |Bounding box of the group.|
|style	            |Style              |	No        |Defines the visual style for the group.|

### Chunk
Chunk is the structure for defining a partial element.

**ATTRIBUTES**

|Attribute         | Data type         | Required |  Description|
| :---             |    :----          |:----     | :---- |
|id                |string             |Yes       |Unique identifier for each group.|
|bounds            |Rectangle          |No        |Bounding box of the group.|
|style             |Style              |No        |Defines the visual style for the group.|

### ExternalItem
ExternalItem is the structure for referencing an external element in a container document.

**ATTRIBUTES**

|Attribute         | Data type         | Required |  Description|
| :---             |    :----          |:----     | :---- |
|id                |string	            |Yes       |Unique identifier for element in container document, e.g., text, image, etc..|

### GroupItem
GroupItem defines the relationship among the items.

**ATTRIBUTES**

|Attribute         | Data type         | Required |  Description|
| :---             |    :----          |:----     | :---- |
|id                |string             |Yes       |Unique identifier for each group.|
|parentID          |string             |No        |Used to define the hierarchical structure for the groups.|
|previousID	      |string             |No        |Links to the previous entry, thus the order of siblings in the group |
|type              | GroupElementType  |Yes       |Defines the type of the group item |


**EXAMPLE: Document structure**
A flat hierarchy is much easier to be edited, because it removes depth from its definition. 
To recover depth and to keep the proper order in its origin, we should know how flat elements relate to each other. 
The first step is to create homogeneous data, then to describe a reference model. 
IDs for the elements are used to reference the items. 
The following JSON example shows the flat hierarchy for the example document introduced in Figure 9 and Figure 10:

```json
{
    "groups": {
        "data": [
            {
                "id": "g0"
            }, {
                "id": "g1"
            }, {
                "id": "g2"
            }, {
                "id": "g3"
            }, {
                "id": "g4"
            }, {
                "id": "g5"
            }, {
                "id": "g6"
            }, {
                "id": "g7"
            }, {
                "id": "g8"
            }, {
                "id": "g9"
            }, {
                "id": "g10"
            }, {
                "id": "g11"
            }
        ], "relations": [
            {
                "id": "g0"
                , "type": "GROUP"
            }, {
                "id": "g1"
                , "parentID": "g0"
                , "type": "GROUP"
            }, {
                "id": "g2"
                , "parentID": "g1"
                , "type": "GROUP"
            }, {
                "id": "g4"
                , "parentID": "g2"
                , "type": "GROUP"
            }, {
                "id": "s0"
                , "parentID": "g4"
                , "type": "SENSOR_DATA"
            }, {
                "id": "g5"
                , "parentID": "g2"
                , "type": "GROUP"
            }, {
                "id": "s1"
                , "parentID": "g5"
                , "type": "SENSOR_DATA"
            }, {
                "id": "s2"
                , "parentID": "g5"
                , "previousID": "s1"
                , "type": "SENSOR_DATA"
            }, {
                "id": "g6"
                , "parentID": "g2"
                , "type": "GROUP"
            }, {
                "id": "s3"
                , "parentID": "g6"
                , "type": "SENSOR_DATA"
            }, {
                "id": "s4"
                , "parentID": "g6"
                , "previousID": "s3"
                , "type": "SENSOR_DATA"
            }, {
                "id": "g3"
                , "parentID": "g2"
                , "type": "GROUP"
            }, {
                "id": "g7"
                , "parentID": "g3"
                , "type": "GROUP"
            }, {
                "id": "s5"
                , "parentID": "g7"
                , "type": "SENSOR_DATA"
            }, {
                "id": "s6"
                , "parentID": "g7"
                , "previousID": "s5"
                , "type": "SENSOR_DATA"
            }, {
                "id": "g8"
                , "parentID": "g3"
                , "type": "GROUP"
            }, {
                "id": "s7"
                , "parentID": "g8"
                , "type": "SENSOR_DATA"
            }, {
                "id": "g9"
                , "parentID": "g3"
                , "type": "GROUP"
            }, {
                "id": "s8"
                , "parentID": "g9"
                , "type": "SENSOR_DATA"
            }, {
                "id": "s9"
                , "parentID": "g9"
                , "previousID": "s8"
                , "type": "SENSOR_DATA"
            }, {
                "id": "s10"
                , "parentID": "g9"
                , "previousID": "s9"
                , "type": "SENSOR_DATA"
            }, {
                "id": "s11"
                , "parentID": "g9"
                , "previousID": "s10"
                , "type": "SENSOR_DATA"
            }, {
                "id": "g10"
                , "parentID": "g3"
                , "type": "GROUP"
            }, {
                "id": "s11"
                , "parentID": "g9"
                , "type": "SENSOR_DATA"
            }, {
                "id": "s12"
                , "parentID": "g10"
                , "previousID": "s8"
                , "type": "SENSOR_DATA"
            }, {
                "id": "g11"
                , "parentID": "g0"
                , "type": "GROUP"
            }, {
                "id": "s13"
                , "parentID": "g11"
                , "type": "SENSOR_DATA"
            }, {
                "id": "s14"
                , "parentID": "g11"
                , "previousID": "s13"
                , "type": "SENSOR_DATA"
            }, {
                "id": "s15"
                , "parentID": "g11"
                , "previousID": "s14"
                , "type": "SENSOR_DATA"
            }, {
                "id": "s16"
                , "parentID": "g11"
                , "previousID": "s15"
                , "type": "SENSOR_DATA"
            }, {
                "id": "s17"
                , "parentID": "g11"
                , "previousID": "s16"
                , "type": "SENSOR_DATA"
            }, {
                "id": "s18"
                , "parentID": "g11"
                , "previousID": "s17"
                , "type": "SENSOR_DATA"
            }, {
                "id": "s19"
                , "parentID": "g11"
                , "previousID": "s18"
                , "type": "SENSOR_DATA"
            }, {
                "id": "s20"
                , "parentID": "g11"
                , "previousID": "s18"
                , "type": "SENSOR_DATA"
            }
        ]
    }
}
```

**EXAMPLE: Chunks**

This example shows how chunks can be used to define groups within an element (see Figure 11), such as single characters referenced in a cursive script.

![Example for chunks](./media/06_chunk.png)

*Figure 11: Example for chunks*

The chunk defines a mask on the relevant sample points. 

```json
{
    "groups": {
        "data": [
            {
                "id": "g0"
            }, {
                "id": "g1"
            }
        ], "chunks": [
            {
                "id": "c0"
                , "pathID": "t0"
                , "type": "SENSOR_DATA_CHUNK"
                , "fromPointIndex": "0"
                , "toPointIndex": "36"
            }, {
                "id": "c1"
                , "pathID": "t0"
                , "type": "SENSOR_DATA_CHUNK"
                , "fromPointIndex": "37"
                , "toPointIndex": "33"
            },
        ], "relations": [
            {
                "id": "g0"
                , "type": "GROUP"
            }, {           
                "id": "g1"
                , "parentID": "g0"
                , "type": "GROUP"
            }, 
            {
                "id": "c0"
                , "parentID": "g1"
                , "type": "CHUNK"
            }, {           
                "id": "g1"
                , "parentID": "g0"
                , "type": "GROUP"
            }, {
                "id": "c1"
                , "parentID": "g1"
                , "type": "CHUNK"
            }
        ]
    }
}
```

## Semantic Metadata
Metadata has been used since the first librarian made a list of the items on a shelf of handwritten scrolls. Metadata means "data about data". Metadata is defined as the data providing information about one or more aspects of the data; it is used to summarise basic information about data which can make tracking and working with specific data easier. In the context of digital ink there are several relevant items of information that need to be shared:

* Means of creation of the data
* Purpose of the data
* Time and date of creation
* Creator or author of the data
* Context where the data was created
* The ink capturing environment where it has been created, such as operating system (OS), firmware of the ink device
* Data quality
* Source of the data

Moreover, linking ink with semantics is another major focus of WILL. 
In order to assign semantics to ink there is a need for logical grouping of ink strokes with a semantic format to describe the meaning of the ink group. 
The Resource Description Framework (RDF) is a family of World Wide Web Consortium (W3C) specifications **[ RDF ]** originally designed as a metadata data model. 
It has come to be used as a general method for conceptual description of information that is implemented in web resources, using a variety of syntax notations and data serialisation formats. 

The RDF data model is similar to classical conceptual modelling approaches (such as entity–relationship or class diagrams). It is based on the idea of making statements about resources (in particular web resources) in expressions of the form subject–predicate–object, known as triples. The subject denotes the resource, and the predicate, also known as the property of the triple, denotes traits or aspects of the resource, and expresses a relationship between the subject and the object.
This basic concept is utilised in our WILL 3.0 format to store the semantic content for ink groups. Figure 11 illustrates how semantic statements are linked to Ink Groups, for the example introduced in Figure 9 the following semantic statements could be formulated:

- **g2** - is_a TEXTLINE
- **g2** - has_text "hello world!"
- **g2** - is http://dbpedia.org/page/%22Hello,_World!%22_program

So, applications receiving this semantically enriched ink can understand the content of Ink document. 

![Example of semantics which are assigned to groups](./media/07_group_tree_semantics.png)

*Figure 11: Example of semantics which are assigned to groups.*

In the following section several elements used to store metadata are introduced. Metadata for digital ink is grouped into device related categories:

* **Document** - Information about the created document
* **Author** - Information about the author of the document
* **Environment** - Information about the capturing environment
* **Ink** - Information about the ink content
* **Device** - Information related to the device


### MetaDataType
Describes the type of ink data.

|Type              | Description                               | 
| :---             |    :----                                  |
|DOCUMENT          |Metadata on document level.                |
|AUTHOR            |Metadata describing the creator / author.  |
|INK               |Metadata describing the content of the ink.|
|ENVIRONMENT       |Metadata describing the environment.       |
|DEVICE            |Metadata describing the ink device.        |

### SemanticTriple
A semantic triple, or simply triple, is the atomic data entity data model. As its name indicates, a triple is a set of three entities that codifies a statement about semantic data in the form of subject predicate object expressions.

**ATTRIBUTES**

|Attribute         | Data type         | Required |  Description                             |
| :---             |    :----          |:----     | :----                                    |
|subject           |string             |Yes       |Subject of the statement, e.g., ink group.| 
|predicate         |string             |Yes       |Predicate of the statement, e.g., is_type.| 
|object            |string             |Yes       |Object of the statement, e.g., Word.      |


### SemanticMetaData
SemanticMetaData is the base structure for storing metadata.

**ATTRIBUTES**

|Attribute         | Data type         | Required |  Description                                |
| :---             |    :----          |:----     | :----                                       |
|type              |MetaDataType       |Yes       |Defined type of metadata.                    |
|statements        |SemanticTriple[]   |Yes       |Statements for the defined meta-data context.|



**Example: Semantics for Figure 9**

```json
{
    "type": "INK"
    , "statements": [
        {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g0"
            , "predicate": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type"
            , "object": "http://ink.wacom.com/will/3.0/inkdocument/Document"
        }, {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g1"
            , "predicate": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type"
            , "object": "http://ink.wacom.com/will/3.0/inkdocument/Paragraph"
        }, {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g2"
            , "predicate": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type"
            , "object": "http://ink.wacom.com/will/3.0/inkdocument/Textline"
        }, {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g3"
            , "predicate": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type"
            , "object": "http://ink.wacom.com/will/3.0/inkdocument/Textline"
        }, {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g4"
            , "predicate": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type"
            , "object": "http://ink.wacom.com/will/3.0/inkdocument/Word"
        }, {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g5"
            , "predicate": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type"
            , "object": "http://ink.wacom.com/will/3.0/inkdocument/Word"
        }, {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g6"
            , "predicate": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type"
            , "object": "http://ink.wacom.com/will/3.0/inkdocument/Punctuation"
        }, {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g7"
            , "predicate": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type"
            , "object": "http://ink.wacom.com/will/3.0/inkdocument/Word"
        }, {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g8"
            , "predicate": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type"
            , "object": "http://ink.wacom.com/will/3.0/inkdocument/Word"
        }, {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g9"
            , "predicate": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type"
            , "object": "http://ink.wacom.com/will/3.0/inkdocument/Word"
        }, {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g10"
            , "predicate": "http://www.w3.org/1999/02/22-rdf-syntax-ns#type"
            , "object": "http://ink.wacom.com/will/3.0/inkdocument/Word"
        }, {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g1"
            , "predicate": "http://ink.wacom.com/will/3.0/has_value"
            , "object": "hello world! this is digital ink"
        }, {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g2"
            , "predicate": "http://ink.wacom.com/will/3.0/has_value"
            , "object": "hello world!"
        }, {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g3"
            , "predicate": "http://ink.wacom.com/will/3.0/has_value"
            , "object": "this is digital ink"
        }, {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g4"
            , "predicate": "http://ink.wacom.com/will/3.0/has_value"
            , "object": "hello"
        }, {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g5"
            , "predicate": "http://ink.wacom.com/will/3.0/has_value"
            , "object": "world"
        }, {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g6"
            , "predicate": "http://ink.wacom.com/will/3.0/has_value"
            , "object": "!"
        }, {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g7"
            , "predicate": "http://ink.wacom.com/will/3.0/has_value"
            , "object": "this"
        }, {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g8"
            , "predicate": "http://ink.wacom.com/will/3.0/has_value"
            , "object": "is"
        }, {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g9"
            , "predicate": "http://ink.wacom.com/will/3.0/has_value"
            , "object": "digital"
        }, {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g10"
            , "predicate": "http://ink.wacom.com/will/3.0/has_value"
            , "object": "ink"
        }, {
            "subject": "http://ink.wacom.com/will/3.0/inkgroup/g11"
            , "predicate": "http://ink.wacom.com/will/3.0/has_value"
            , "object": "globe"
        }
    ]
}
```

# Normative Reference
WILL 3.0 has a major focus on interoperability and the use of existing established standards. This section provides references to existing standards relevant in the context of WILL 3.0.

## Ink formats
**[ InkML ]**  Ink Markup Language (InkML)      

[http://www.w3.org/TR/InkML/](http://www.w3.org/TR/InkML/)

**[ ISF ]** Ink Serialized Format (ISF)

[http://download.microsoft.com/download/0/B/E/0BE8BDD7-E5E8-422A-ABFD-4342ED7AD886/InkSerializedFormat(ISF)Specification.pdf](ISF Specification)

[ WILL ] Wacom Ink Layer Language (WILL), Version 1. 

https://developer-docs.wacom.com/display/DevDocs/Format+Specification

## Graphics format
**[ SVG ]** Scalable Vector Graphics (SVG) 1.1 (Second Edition)

http://www.w3.org/TR/SVG/

**[ PDF ]** ISO 32000-1:2008 Document management -- Portable document format -- Part 1: PDF 1.7

https://www.iso.org/standard/51502.html

## Semantic formats
**[ RDF ]** RDF/XML Syntax Specification (Revised), D. Beckett, Editor, W3C Recommendation, 10 February 2004,

http://www.w3.org/TR/2004/REC-rdf-syntax-grammar-20040210/

**[ DCES ]** Dublin Core Metadata Element Set, Version 1.1. Dublin Core Metadata Initiative. 

http://dublincore.org/documents/dces/


# Scientific References
**[  James 12 ]** James, K. H., & Engelhardt, L. (2012). The effects of handwriting experience on functional brain development in pre-literate children. Trends in Neuroscience and Education, 1(1), 32–42. *https://doi.org/10.1016/j.tine.2012.08.001*

**[  James 17 ]** James, K. H. (2017). The Importance of Handwriting Experience on the Development of the Literate Brain. Current Directions in Psychological Science, 26(6), 502–508. *https://doi.org/10.1177/0963721417709821*

**[ Douglas 73]** David H. Douglas  Thomas K. Peucker, Algorithms for the Reduction of the Number of Points Required to Represent a Digitized Line or its Caricature, Cartographica: The International Journal for Geographic Information and Geovisualization 1973 10:2, 112-122 

# Annex A.  (Informative) Serialisation  
```protobuf
syntax = "proto3";

package WacomInkFormat3;

option optimize_for = SPEED;
option java_multiple_files = true;
option java_package = "com.wacom.ink";

// -------------------------- Math types -------------------------------------------------------------------------------

/**
 *	Representation of a three dimensional vector.
 */
message Vector3 {
	float x = 1;
	float y = 2;
	float z = 3;
}

/**
 *	Representation of a 3x3 homogeneous matrix
 *
 *	| m00  m01  m02 |
 *	| m10  m11  m12 |
 *	| m20  m21  m22 |
 */
message Matrix3 {
	float m00 = 1;
	float m01 = 2;
	float m02 = 3;
	float m10 = 5;
	float m11 = 6;
	float m12 = 7;
	float m20 = 9;
	float m21 = 10;
	float m22 = 11;
}

/**
 *	Representation of a 4x4 affine matrix
 *
 *	| m00  m01  m02  m03 |
 *	| m10  m11  m12  m13 |
 *	| m20  m21  m22  m23 |
 *	| m30  m31  m32  m33 |
 */
message Matrix4 {
	float m00 = 1;
	float m01 = 2;
	float m02 = 3;
	float m03 = 4;
	float m10 = 5;
	float m11 = 6;
	float m12 = 7;
	float m13 = 8;
	float m20 = 9;
	float m21 = 10;
	float m22 = 11;
	float m23 = 12;
	float m30 = 13;
	float m31 = 14;
	float m32 = 15;
	float m33 = 16;
}

/**
 * Representation of rectangle
 */
message Rectangle {
	float x = 1;
	float y = 2;
	float width = 3;
	float height = 4;
}

// -------------------------- Input Enums ------------------------------------------------------------------------------
/**
 * The ink device state defines the state of the Ink device.
 * WILL 3.0 supports different modes:
 *  - Writing on a plane
 *  - Hovering above a surface
 *  - Moving in air (VR/AR/MR) interaction
 *  - Only hovering in the air
 */
enum InkState {
	PLANE = 0;               // Ink device is writing on a surface.
	HOVERING = 1;            // Hovering
	IN_VOLUME = 2;           // Using the ink device in the air, with active inking
	VOLUME_HOVERING = 3;     // Moving the pen in the air with disabled inking
	START_TRACKING = 4;      // For Hovering and VR entering the proximity sensor needs to be flagged, as well ...
	STOP_TRACKING = 5;       // as leaving the proximity sensor respectively VR tracking.
}

/**
 * Ink sensor types.
 */
enum InkSensorType {
	CUSTOM = 0;              // Custom channel, could be button state, additional sensor, such as camera, microphone, ...
	X = 1;                   // X coordinate. This is the horizontal pen position on the writing surface.
	Y = 2;                   // Y coordinate. This is the vertical position on the writing surface.
	Z = 3;                   // Z coordinate. This is the height of pen above the writing surface.
	TIMESTAMP = 4;           // Time (of the sample point)
	PRESSURE = 5;            // Pressure value
	RADIUS_X = 6;            // Touch radius by X
	RADIUS_Y = 7;            // Touch radius by Y
	AZIMUTH	= 8;             // Azimuth angle of the pen (yaw)
	ALTITUDE = 9;            // Elevation angle of the pen (pitch)
	ROTATION = 10;           // Rotation (counter-clockwise rotation about pen axis)
}

/**
 * All supported unit types are defined in an enum
 */
enum InkSensorMetricType {
	LENGTH = 0;             // underling si unit is meter
	TIME = 1;               // underling si unit is second
	FORCE = 2;              // underling si unit is newton
	ANGLE = 3;              // underling si unit is radian
	NORMALIZED = 4;         // percentage, expressed as a fraction (1.0 = 100%) relative to max-min
	LOGICAL_VALUE = 5;
}

/**
 * Data type used to store sample points.
 * Values are stored in little-endian form unless otherwise specified.
 */
enum DataType {
	BOOLEAN = 0;                                         // Byte-size values
	FLOAT32 = 1;                                         // 32-bit floating-point significand (exponent defined by "precision" member of DataList)
	FLOAT64 = 2;                                         // 64-bit floating-point significand (exponent defined by "precision" member of DataList)
	INT32 = 3;
	INT64 = 4;
	UINT32 = 5;
	UINT64 = 6;
}

/**
 * Method used to encode sample point storage.
 */
enum EncodingMethod {
	DIRECT = 0;                                          // Values are directly stored according to their DataType
	DELTA = 1;                                           // Differences in directly-encoded values are stored
}

// -------------------------- Ink Device Meta Data ------------------------------------
/**
 * Definition of the ink device.
 */
message InkDevice {
	string id = 1;
	string manufacturer = 2;                             // Manufacturer of the device
	string model = 3;                                    // Model name of the device
	string serialNumber = 4;
	string manufacturerID = 5;                           // Globally unique identifier for device (e.g., Wacom Pen ID, MAC-Address, serial number, ...)
	repeated InkSensorChannelGroup channelGroups = 6;    // Sensor channels defined for Ink device
}

message InkSensorChannelGroup {
	string id = 1;                               // Group that provides X and Y channels is the one that is reffered from PathRelation and it's id could be always XY
	repeated InkSensorChannel channels = 2;      // Channels in this group provides aligned values in the same time
	uint32 samplingRateHint = 3;                 // Hint for the intended sampling rate of the channel [Optional]
	uint32 latency = 4;                          // Latency measure in milliseconds [Optional]
}

/**
 * Information about the sensor channel.
 */
message InkSensorChannel {
	string id = 1;
	InkSensorType type = 2;                      // Type of the sensor.
	string name = 3;                             // Name of the channel. Predefined channels are represented in the sensor channel constants.
	InkSensorMetricType metric = 4;              // Indicates metric used in calculating the resolution for the data item.
	float resolution = 5;                        // Is a decimal number giving the number of data item increments
                                                 // per physical unit., e.g. if the physical unit is in m and device units
                                                 // resolution is 100000, then the value 150 would be 0.0015 m.
	float min = 6;                               // Minimal value of the channel
	float max = 7;                               // Maximal value of the channel
}

// -------------------------- RAW Sensor Data --------------------------------------------------------------------------
/**
 * List of data items.
 */
message DataList {
	DataType type = 1;                           // Type of the data list
	bytes data = 2;                              // Compressed sample values
	string channelID = 3;                        // Referencing InkSensorChannel via id
	uint32 precision = 4;                        // Precision of integer encoding, needed for encoded float values.
	EncodingMethod encoding = 5;                 // Algorithm used to encode or compress data
}

/**
 * Adding additional sensor events, for instance from a button press.
 */
message SensorEvent {
	string id = 1;
	uint64 timestamp = 2;                        // Milliseconds
	string type = 3;                             // Specification of the event type
	string value = 4;                            // Value of the event
	string reference = 5;                        // [optional] Reference to an external event description file (e.g. if the sensor is camera)
}

/**
 * Capturing context of the ink device.
 */
message InkDeviceContext {
	string id = 1;                               // ID of the context to reference with meta data
	string inkDeviceID = 2;                      // Referencing the InkDevice object via id
	Vector3 offset = 3;                          // Used for the relative input mode, defining the origin of the coordinate system
}

/**
 * SensorData is the central data structure for storing raw data from the ink sensor.
 */
message SensorData {
	string id = 1;
	string inkDeviceContextID = 2;               // Referencing the InkDeviceContext via id
	InkState state = 3;                          // State of the ink
	uint64 timestamp = 4;                        // Timestamp for first sample of the stroke, measured in milliseconds
	repeated DataList dataChannels = 5;          // Data channels encoding the data list
}

// -------------------------- Ink Geometry --------------------------------------------------------------------
/**
 * Visual path structure.
 * Path in its simplest form contains one or more Segments (Catmull-Rom), which includes a series of positional points,
 * Positions.
 * In addition to those properties, digital ink in WILL Data Format includes Start Parameter and End Parameter as
 * Path entity's properties.
 */
message Path {
	string id = 1;

	float startParameter = 2;                 // Spline start parameter [0, 1]
	float endParameter = 3;                   // Spline end parameter [0, 1]

	// uint32 precision = 4;                  // Precision value used for integer differential encoding

	repeated float splineX = 4;               // Spline values for x
	repeated float splineY = 5;               // Spline values for y
	repeated float splineZ = 6;               // Spline values for z [3D rendering]
	repeated bytes red = 7;                   // Color values for red channel
	repeated bytes green = 8;                 // Color values for green channel
	repeated bytes blue = 9;                  // Color values for blue channel
	repeated bytes alpha = 10;                // Color values for alpha channel
	repeated float size = 11;                 // Brush size
	repeated float rotation = 12;             // Brush rotation
	repeated float scaleX = 13;               // Brush scale x
	repeated float scaleY = 14;               // Brush scale y
	repeated float scaleZ = 15;               // Brush scale z [3D rendering]
	repeated float offsetX = 16;              // Brush offset x
	repeated float offsetY = 17;              // Brush offset y
	repeated float offsetZ = 18;              // Brush offset z [3D rendering]

	repeated uint32 sensorDataMapping = 19;   // Explicit mapping between indices of Path and SensorData, used when input rate is very high and provides unwanted points
}

// -------------------------- Meta data structures ---------------------------------------------------------------------

// -------------------------- Brush Configuration ------------------------------------
enum RotationMode {
	NONE = 0;
	RANDOM = 1;
	TRAJECTORY = 2;
}

/**
 * A brush defined with a convex polygon (convex polyhedron for 3D)
 */
message VectorBrush {
	string id = 1;
	repeated float pointX = 2;
	repeated float pointY = 3;
	repeated float pointZ = 4;                   // [3D rendering]
	repeated uint32 indices = 5;                 // [3D rendering]
}

/**
 * A brush defined with raster images
 */
message RasterBrush {
	string id = 1;
	repeated bytes shapeTexture = 2;
	float spacing = 3;
	float scattering = 4;
	RotationMode rotation = 5;
	repeated bytes fillTexture = 6;
	float fillWidth = 7;
	float fillHeight = 8;
	bool randomizeFill = 9;
}

// -------------------------- Groups Configuration ------------------------------------
/**
 * Defines the element of the group.
 */
enum GroupElementType {
	GROUP = 0;
	PATH = 1;
	SENSOR_DATA = 2;
	CHUNK = 3;
	EXTERNAL = 4;
}

enum ChunkType {
	PATH_CHUNK = 0;
	SENSOR_DATA_CHUNK = 1;
}

/**
 * Group structure defining the id of the group as required element.
 * Moreover, optionally its rectangle and a defined style can be stored.
 */
message Group {
	string id = 1;                      // Group id, which is used in the GroupItem in the parentID
	Rectangle bounds = 2;               // Bounding box of the group [optional]
	Style style = 3;                    // Defined visual style for the group [optional]
}

/**
 * Describes path part
 */
message Chunk {
	string id = 1;
	string pathID = 2;                  // path reference
	ChunkType type = 3;                 // defines the type of the chunk
	uint32 fromPointIndex = 4;          // chunk beginning
	uint32 toPointIndex = 5;            // chunk ending
}

/**
 * References an external item such as image, text, or similar in the container document.
 */
message ExternalItem {
	string id = 1;
}

/**
 * A group consists of several group items which have a specific group element type.
 */
message GroupItem {
	string id = 1;                       // Reference to the id of the element, e.g., path or sensorData id
	string parentID = 2;                 // Used to define the hierachical structure for the groups
	string previousID = 3;               // Links to the previous entry
	GroupElementType type = 4;           // Defines the type of the group item
}

/**
 * Groups flats hierarchy and describes how to recover it.
 * Relations are needed to be possible this recovering.
 * Flat hierarchy is much easier to be edited, because removes depth from it's definition.
 *
 * For example if you have:
 *
 *		<Group id="1">
 *			<Path id="2" />
 *			<Group id="3">
 *				<Path id="4" />
 *				<Path id="5" />
 *				<Path id="6" />
 *			</Group>
 *			<Path id="7" />
 *			<Path id="8" />
 *			<Group id="9">
 *				<Path id="10" />
 *				<Group id="11">
 *					<Path id="12" />
 *					<Path id="13" />
 *				</Group>
 *				<Path id="14" />
 *				<Path id="15" />
 *			</Group>
 *		</Group>
 *
 * then flatten will be:
 *
 *		<Group id="1" />
 *		<Path id="2" />
 *		<Group id="3" />
 *		<Path id="4" />
 *		<Path id="5" />
 *		<Path id="6" />
 *		<Path id="7" />
 *		<Path id="8" />
 *		<Group id="9" />
 *		<Path id="10" />
 *		<Group id="11" />
 *		<Path id="12" />
 *		<Path id="13" />
 *		<Path id="14" />
 *		<Path id="15" />
 *
 * to recover depth and to keep proper order in it's origin, then we should know how flat elements relates to each other.
 * First step is to create homogeneous data, then to describe reference model. We use ids from already avilable data messages.
 *
 *		<GroupItem type="Group" id="1" />
 *		<GroupItem type="Path" id="2" parentID="1" />
 *		<GroupItem type="Group" id="3" parentID="1" previousID="2" />
 *		<GroupItem type="Path" id="4" parentID="3" />
 *		<GroupItem type="Path" id="5" parentID="3" previousID="4" />
 *		<GroupItem type="Path" id="6" parentID="3" previousID="5" />
 *		<GroupItem type="Path" id="7" parentID="1" previousID="3" />
 *		<GroupItem type="Path" id="8" parentID="1" previousID="7" />
 *		<GroupItem type="Group" id="9" parentID="1" previousID="8" />
 *		<GroupItem type="Path" id="10" parentID="9" />
 *		<GroupItem type="Group" id="11" parentID="9" previousID="10" />
 *		<GroupItem type="Path" id="12" parentID="11" />
 *		<GroupItem type="Path" id="13" parentID="11" previousID="12" />
 *		<GroupItem type="Path" id="14" parentID="9" previousID="11" />
 *		<GroupItem type="Path" id="15" parentID="9" previousID="14" />
 *
 * When we have this data structure then natural order doesn't matter and we can easily add stroke in group with id 9 after stroke with id 15 for example.
 * Because data is already homogeneous we could have and other elements that are part from the hierarchy.
 * Text and Images are handled from items with type EXTERNAL. They are not subject of this format but our structure is able to handle them.
 */
message Groups {
	repeated Group data = 1;
	repeated GroupItem relations = 2;
	repeated Chunk chunks = 3;
	repeated ExternalItem externalItems = 4;
}

// -------------------------- Style Configuration ------------------------------------
/**
 * Visual settings of processed path
 */
message Style {
	float width = 1;                             // Static with of stroke when dynamic is not needed
	fixed32 color = 2;                           // Stroke color
	string brushID = 3;                          // Reference to Brush used for stroke rasterization
	uint32 particlesRandomSeed = 4;              // Particles brush seed
	string renderMode = 5;                       // Rendering semantic which describes how to interpret stroke data, different than regular stroke, for example ERASE
}

// -------------------------- Semantic Meta Data structures ------------------------------------------------------------

/**
 * Definition of the metadata type.
 *  - Document: Storing meta data regarding the document
 *  - Author: Storing meta data about the author of a document
 *  - Ink: Ink semantics
 */
enum MetaDataType {
	DOCUMENT = 0;
	AUTHOR = 1;
	INK = 2;
	ENVIRONMENT = 3;
	DEVICE = 4;
}

/**
 * A semantic triple, or simply triple, is the atomic data entity data model. As its name indicates, a triple is a set
 * of three entities that codifies a statement about semantic data in the form of subject predicate object expressions.
 */
message SemanticTriple {
	string subject = 1;
	string predicate = 2;
	string object = 3;
}

/**
 * Metadata structure
 */
message SemanticMetaData {
	MetaDataType type = 1;                     // Defines the type of meta data
	repeated SemanticTriple statements = 2;    // Semantic messages
}

// -------------------------- Relation model ---------------------------------------------------------------------------
/**
 * Lists all devices used within the ink document.
 */
message Devices {
	repeated InkDevice data = 1;
}

/**
 * Contains all device related data:
 * - Recording of ink contexts,
 * - additional sensor events.
 */
message DeviceData {
	repeated InkDeviceContext contexts = 1;      // Defined contexts
	repeated SensorEvent events = 2;             // Additional sensor events [Optional]
}

/**
 * Contains all semantic meta data.
 */
message Semantics {
	repeated SemanticMetaData data = 1;
}
/**
 * Brush descriptions, needed for ink rasterization
 */
message Brushes {
	repeated VectorBrush vectored = 1;
	repeated RasterBrush rastered = 2;
}

/**
 * Relation model of raw, processed and visual path
 */
message PathRelation {
	uint32 sensorDataOffset = 1;                  // Index of points mapping between raw and processed paths
	string sensorDataID = 2;                      // Reference to raw data, src of processed path
	string pathID = 3;                            // Reference to processed path
	Style style = 4;                              // Visual settings of processed path
}

/**
 * Contains all ink data.
 */
message InkData {
	repeated Path paths = 1;                      // All digital ink paths
	repeated SensorData sensorData = 2;           // List of sensor data items
	repeated PathRelation relations = 3;          // Links RAW data to paths
	Groups groups = 4;                            // All defined groups
}

// -------------------------- Universal Ink ----------------------------------------------------------------------------
/**
 * Universal Ink as base structure to capture all relevant data.
 * - Sensor data
 */
message UniversalInkData {
	DeviceData deviceData = 1;      // Relevant device data
	InkData inkData = 2;            // Digital Ink data
	Brushes brushes = 3;            // Rasterizing tools
	Semantics semantics = 4;        // Authors, semantics, document
	Devices devices = 5;            // List of devices
}
```
# Annex B: Implementation Guidelines
The following are informative implementation guidelines for reducing WILL 3.0 file size and environmental interactions.

- **Gzip compression.** The lossless gzip compression [ RFC1952 ] will help to reduce the WILL 3.0 file size considerably.  It is recommend that applications have the facility to compress and decompress WILL 3.0 files and streams using the gzip algorithm.
