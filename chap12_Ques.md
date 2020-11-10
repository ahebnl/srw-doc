# **Frequently Asked Questions**

## **How to Import Magnetic Field Data into SRW ?**

The simplest method to import an external Magnetic Field data into SRW is to use the macro
**SrwMagImportCmpn** (menu call
"(Arbitrary) Magnetic Field->Modify Component->Import..."). If this macro failed to import your
data, your can try to do the import "manually", after reading the following section.

There is a number of ways one can import the values of the vertical and horizontal fields as a
function of the longitudinal coordinate into SRW for a further processing. We assume here that
you are familiar with Igor, at least that you know how to graph or edit a wave. If it is not the case,
we urge you to experience the Igor tutorials which will introduce you to a number of capabilities
of Igor. If you need to import the field data episodically, you can use the Method#1 or 2
described below. If you want to do it many times, the best is that you write your own macro for
that, then you should concentrate on the Method#2.

**Before you start**
- You have to prepare your field data as two lists of field values corresponding to equidistant
spacing of the longitudinal position. One is the vertical field component, the other is the
horizontal one. Make sure that you have the **same number of points** for both field components
and that the data of the two lists corresponds to the same longitudinal position. For debugging
purposes, it is easier to deal with the field data in ASCII (Text) format rather than Binary. It is a
good practice to watch your data in Microsoft Excel, because then it can be easily copied,
manipulated and pasted into Igor and you have a full visual control on it.
- Next, you have to understand the way SRW is describing the magnetic field data. For each
description of the magnetic field, SRW creates one text wave and two waves of double precision
real numbers. The text wave holds the names of the two other waves together with some other
information. The two real waves hold the vertical and horizontal field data.

**Method#1**: Your data is available in Text mode and you have Microsoft Excel available. The
easiest (but difficult to automatize) way to import your data is the following.
1. Open your data in Microsoft Excel and organize it into two vertical columns, one for the
vertical field and the other for the horizontal field.
2. Start Igor, Initialize SRW and execute the macro **SrwMagFieldCreate** (menu call
"(Arbitrary) Magnetic Field->Create and Modify..."). In the dialog box, enter a name, followed by
the longitudinal coordinate of the center of your field data (0 is a good default value), followed by
the total range in meters of the longitudinal coordinate covered by your field data, followed by
the number of points in each field components (the same as the number of points in Excel).
After execution, all three waves have been generated. If you have entered the name "MyData" in
the dialog box of the macro **SrwMagFieldCreate**, then the waves "MyData_mag" (text wave),
"MyDataBX_fld" (horizontal field) and "MyDataBZ_fld" (vertical field) have been created.
"MyDataBX_fld" and "MyDataBZ_fld" have been initialized to 0.
3. Edit the waves "MyDataBX_fld" and "MyDataBZ_fld" one by one either from the Igor menu or by typing "Edit MyDataBX_fld MyDataBZ_fld" in the Command Window, Select the whole
column holding the data of the wave and Erase it (Cut from the Menu Edit). Select the vertical
field data in Microsoft Excel and Paste it into "MyDataBZ_fld" in the window being edited.
Continue with the horizontal field (Paste data into "MyDataBX_fld").
4. Convert your field data to Tesla units by multiplying "MyDataBX_fld" and "MyDataBZ_fld" by a
proper constant. For example, if your data are in Gauss, you should divide it by 10000. Type for
this "MyDataBX_fld = MyDataBX_fld/10000" and "MyDataBZ_fld = MyDataBZ_fld/10000" in the
Command Window.
5. The job is done. Check that everything is correct by executing the SRW macro
SrwMagDisplayField("MyDataBX_fld") and SrwMagDisplayField("MyDataBZ_fld") or by typing
"Display MyDataBZ_fld" and "Display MyDataBX_fld" in the Command Window. Check that the
curves together with the units of both axes are correct.

**Method#2**: You do not want to copy/paste between Igor and Excel either because you have no
Excel or because you want to automatize the process using macro commands of Igor.
1. You have to first study and experience various modes of importing waves into Igor and test
them on your data until it works. See the Igor documentation for this. Depending on your data
structure, it may be more or less easy do (Igor is rather smart so do not be afraid; the problems
can come only with some special binary data format). At the end you should have in memory
two waves, say "Bz" and "Bx" (or any other name that you have given) that holds your data. If
you do not succeed, we advise you to convert your data into ASCII (Text) format using some
other utilities and load it into Igor with the "Load General Text..." or "Load Delimited Text..."
commands.
2. Set the longitudinal scale of "Bx" and "Bz" waves by the Menu "Data", sub-menu "Change
Wave Scaling" of Igor. Display them in Igor (for example by typing "Display Bz" and "Display Bx"
in the Command Window or through the Windows menu of Igor).
3. Do all the steps of the Method#1 except for the Copy/Paste between Igor and Excel.
4. Copy the content of the "Bx" and "Bz" waves into "MyDataBX_fld" and "MyDataBZ_fld" by
typing "MyDataBZ_fld=Bz" and "MyDataBX_fld=Bx" in the Command Window.
5. Convert the field units in Tesla (if needed) as described in the Method#1.
6. Check the result by typing "Display MyDataBZ_fld" and "Display MyDataBX_fld" in the
Command Window.
7. Watch the content of the History Window. It holds the text form of all the commands that you
have invoked in these operations. Copying and editing its content into a procedure file is a good
way to write a Macro to automatize this task.

**More Remarks**
- If your field data points do not correspond to equidistant longitudinal coordinates, you can
process them with the "Interpolate" macro of Igor. For this you have to import both the field data
and the associated longitudinal coordinate of the points and run the "Interpolate" macro with the
proper settings. You will obtain one wave for the vertical field and another wave for the
horizontal field that you can further process according to the Method#2. Make sure that the
vertical and horizontal field is interpolated for the same longitudinal coordinates.
- If you are an experienced user of Igor, we advise you to print and watch various macros where
the SRW field data is created and manipulated (file "SRW MagField Gen.ipf" in the SRW
Procedure folder).
## **Why electron trajectory appears tilted in an undulator and / or why there is no UR central cone at zero direction ?**
SRW computes the electron trajectory from the following input data.

Electron Beam structure:
- electron beam energy;
- initial horizontal and vertical positions and angles of the filament electron beam specified at
some longitudinal position.

Magnetic Field structure:
- horizontal and vertical magnetic field components tabulated vs longitudinal position and stored
in 1D numerical waves.

If you have checked that your Magnetic Field structure is defined properly (at least, magnetic
field plots created from menu "...Arbitrary Magnetic Field -> Display Field..." look correct), than
the most probable reason of the trajectory tilt is improper definition of the initial horizontal and
vertical electron positions / angles or the longitudinal position to which they relate (these
parameters are entered in the dialog "Electron Beam" or by the macro **SrwElecFilament**).

For example, it is not at all always correct to assume that the transverse positions and angles of
the electron trajectory are zero in the middle of an undulator (typically at zero longitudinal
position). For an undulator structure with zero first field integral, it is much more safe to assume
that the transverse positions and angles of the electron trajectory are zero at some longitudinal
position before the undulator (yet not out of the magnetic field definition range!).

If the first field integral is not zero in your undulator, you can change (reduce) the trajectory tilt by
varying the value(s) of the initial horizontal (vertical) angle(s) of the filament electron beam using
the dialog "Electron Beam" or the macro **SrwElecFilament**.

## **How can I take into account finite electron beam emittance at SR propagation ?**
In the present version of the code, the finite e-beam emittance can be taken into account in the
following way.

The features of your beamline may allow to accept that the intensity distribution due to the finite
e-beam emittance is a result of convolution of the intensity distribution computed for the filament
e-beam with the particle density distribution in the real e-beam. This happens, for example, in
simplest schemes of the SR focusing. In any particular case, one can test whether this takes
place or not by repeating the propagation for the filament e-beam with a small displacement in
initial transverse coordinate and/or angle within the electron beam size and divergence, and
watching whether it results only in a shift of the intensity profile after the propagation, or the
shape of the profile is also affected. In the former case, one is more-or-less safe to assume the
convolution formalism to be valid, and one can easily extract the intensity distribution due to finiteemittance
electron beam using the SRW menu "Visualize...", by choosing "Multi-e Intensity"
under "Single-e / Multi-e" field in the dialog box (for more details, please see the help for the
macro **SrwWfr2Int**).

If the convolution formalism can not be applied, you can try to determine the multi-electron
intensity by summing-up and averaging the intensity distributions obtained for the filament ebeam
with different initial transverse positions and/or angles which satisfy the particle density
distribution of the real e-beam (typically, Gaussian distribution is a good approximation). At this
procedure, it may appear more CPU-efficient to move the beamline transversely with respect to
the e-beam, than vice versa.

## **How can I simulate an optical element which is not yet implemented in SRW ?**
In SRW starting from version 3.2.0, there is a "Generic Thin" optical element implemented. It
approximates the transformation of the electric field from the transverse plane before the optical elements to the transverse plane immediately after it by multiplication of the field at any point of
the wavefront by a 2D complex transmission function which, in general case, modifies both the
phase and amplitude of the electric field.

If the "Thin" approximation is appropriate for your optical element, you need to do the following
to simulate it.
1. Write in Igor Pro macro language two scalar functions of two scalar arguments, one of which
returns the Optical Path Difference and the other one the Amplitude Transmission for any
transverse position (two transverse coordinates in natural frame) at propagation from the
transverse plane before the optical element to the plane immediately after it.
2. Execute the macro **SrwOptThinTempl** (menu SRWP "Optical Element: Thin ->
Transmission: Template...") which will create a template of a "Thin" element, i.e. optical element
structure and a 2D complex wave to keep information on the Optical Path Difference and
Amplitude Transmission. In this macro, you should give a name to your element and specify the
transverse dimensions and center position for it.
3. To finish the setup of the optical element, execute the macro SrwOptThinSetup (menu SRWP
"Optical Element: Thin -> Transmission: Setup..."), which will fill the 2D complex wave created at
the step 2, using the Optical Path Difference and Amplitude Transmission functions prepared at
the step 1.

Refractive Lenses for X-rays are implemented in IGOR part of the SRW using the mechanism
described above. Have a look at the SRW procedure file where these elements are
implemented. You can open this file using IGOR menu "Windows -> Procedure Windows ->
SRW Optics ThinGen.ipf".

Alternatively, you can execute only the step 2 (i.e. create template with empty 2D complex
wave), and then fill the 2D complex wave, using the IGOR macro language, with the data
describing your optical element. *Real part of each point of this 2D complex wave should be the
Amplitude Transmission, and imaginary part of it should be the Optical Path Difference
introduced by the optical element (<Optical Path Difference> = <Phase Shift>/<Wave
Number>).*

You can check the results of setting up your optical element by displaying its characteristics
(amplitude or intensity transmission, or optical path) by using the macro
**SrwOptThinTransmDisplay** (menu SRWP: "Optical Element: Thin -> Transmission: Display...")

 


