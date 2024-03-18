#########################################
M1M3 actuator delays and following errors
#########################################

Abstract
========
   This study looks at the delays between the actuator commanded forces and the measured forces.  For bump tests and "gentle" slews, the measured forces match the applied forces relatively well, with a delay on the order of 100ms.  However, for more aggressive slews, the measured forces do not track the applied forces well at all, indicating that the problem with large hardpoint forces for the aggressive slews is more serious than just time delays.

Delays calculated from bump tests
=====================================
The bump test involves applying a force profile to one actuator at a time to test the actuator response.  Much more detail can be found at https://sitcomtn-083.lsst.io/.  To evaluate the delay between the applied force and the measured force, the measured force is shifted in order to minimize the sum of squared differences between the measured and applied forces.  In Figure 1, it can be seen that the measured force tracks the applied force well, and that the measured force lags the applied force by about 100ms.  Figure 2 shows a histogram of the delays measured on all 156 actuators.

.. image:: ./_static/Bump_Test_Delays_212.png

Figure 1.  Bump test applied and measured forces, and the delay required to align the two.

.. image:: ./_static/Bump_Test_Delay_Histograms_20240104T100658.png

Figure 2.  Histogram of delays for all 156 actuators.

Delays from "gentle" slews
=====================================
The bump test is a relatively simple test, so the next step was to evaluate the delays during actual slews of the TMA.  The first set of tests I looked at were relatively gentle slews.  These were run with 40% TMA speeds and reduced jerks, so the limits were as follows:

* Azimuth Velocity - 4.0 degrees/sec
* Azimuth acceleration - 4.0 degrees/sec^2
* Azimuth jerk - 2.0 degrees/sec^3
* Elevation Velocity - 2.0 degrees/sec
* Elevation acceleration - 2.0 degrees/sec^2
* Elevation jerk - 1.0 degrees/sec^3

With these values, Figure 3 shows two slews from two different nights, and Figure 4 shows the applied ane measured forces during these slews.  The measured slews are noisier, but it can still be seen that the measured forces track the applied forces relatively well, and the delays are similar to what was seen in the bump tests.

.. image:: ./_static/20240105_1047.png 
   :width: 49%
.. image:: ./_static/20240109_147.png 
   :width: 49%

Figure 3.  Hardpoint /TMA plots from two gentle slews on two different nights.



.. image:: ./_static/Slew_Delays_20240105_1047_108.png
   :width: 49%
.. image:: ./_static/Slew_Delays_20240105_1047_320.png
   :width: 49%
.. image:: ./_static/Slew_Delays_20240109_147_108.png
   :width: 49%
.. image:: ./_static/Slew_Delays_20240109_147_320.png
   :width: 49%

Figure 4.  Applied vs measured actuator forces for two gentle slews on two different nights.


Delays from more aggressive slews.
=====================================
In actual operation, we need to slew the TMA more aggressively.  For this set of tests, the TMA was slewed at 70% of full speed, which has the limits as follows:

* Azimuth Velocity - 7.0 degrees/sec
* Azimuth acceleration - 7.0 degrees/sec^2
* Azimuth jerk - 28.0 degrees/sec^3
* Elevation Velocity - 3.5 degrees/sec
* Elevation acceleration - 3.5 degrees/sec^2
* Elevation jerk - 14.0 degrees/sec^3

With these values, Figure 4 shows two slews from two different nights, and Figure 5 shows the applied ane measured forces during these slews.  Here it can be seen that the measured forces are not tracking the applied forces well at all.  Clearly the problem with the measured forces is more serious than simply a delay relative to the applied forces.  Note that with such large differences in shape between the measured and applied forces, the delay calculated by attempting to align these two different shapes is probably meaningless.


.. image:: ./_static/20240102_607.png 
   :width: 49%
.. image:: ./_static/20240103_1297.png 
   :width: 49%

Figure 5.  Hardpoint /TMA plots from two aggressive slews on two different nights.



.. image:: ./_static/Slew_Delays_20240102_607_108.png
   :width: 49%
.. image:: ./_static/Slew_Delays_20240102_607_320.png
   :width: 49%
.. image:: ./_static/Slew_Delays_20240103_1297_108.png
   :width: 49%
.. image:: ./_static/Slew_Delays_20240103_1297_320.png
   :width: 49%

Figure 6.  Applied vs measured actuator forces for two aggressive slews on two different nights.

All of the delay plots shown here were generated with the notebook at:
https://github.com/lsst-sitcom/notebooks_vandv/blob/develop/notebooks/tel_and_site/subsys_req_ver/m1m3/SITCOMTN-083_fa_Error_lag_analysis.ipynb

Movies of force actuator following errors
=====================================================
To better understand the deviations of the mesaured forces from the commanded forces, code was developed showing the force actuator following errors as a function of time and actuator position.  Two of these movies, one for a gentle slew and one for a more aggressive slew, are stored in the "movies" directory at the github location for this technote. (https://github.com/lsst-sitcom/sitcomtn-107). The notebook to make these movies is called SITCOMTN-107_Actuator_Following_Error_Movie_18Mar24.ipynb, and is in the "notebooks" directory at that same location.

Figure 7 shows single frames of the movie of the more aggressive slews.  On the left, at a time when the hardpoint forces are exceeding the limit, it can be seen that the following errors are large.  On the right, when the hardpoint forces are within the limits, the following errors are much smaller.


.. image:: ./_static/Movie_Large_HP_Forces.png
   :width: 49%
.. image:: ./_static/Movie_Small_HP_Forces.png
   :width: 49%

Figure 7.  Single frames of the movie of an aggressive slews (20240102 - 1308).  On the left, at a time when the hardpoint forces are exceeding the limit, it can be seen that the following errors are large.  On the right, when the hardpoint forces are within the limits, the following errors are much smaller.  The single dark actuator at the top was disabled at the time of this test.

Also, note that  the actuators with large following errors are primarily around the edge, perhaps because the forces are larger there.  It is hoped that these movies will help us understand and fix what is causing the large discrepancies in the measured forces.

Conclusions
=====================================
For aggressive slews, the measured forces do not track the applied forces well at all.  So it appears that the problem with large hardpoint forces for the aggressive slews is more serious than just time delays.  We need to understand why the measured forces are deviating so strongly from the intended forces.
