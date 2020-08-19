# 3D-Launch-Sim
3D launch trajectory simulation tool

This program computes the ascent trajectory of a launch vehicle in an Earth Centered Inertial (ECI) frame of reference. The launch site is selected based on the longitude and latitude, from which the state equations are numerically integrated using a first order Euler method. 

Thrust and drag forces are computed in the local North-East-Up frame of reference which defines the pitch angle from the local horizontal, as well as the yaw angle from north. These force vectors are transformed into the ECI frame through a coordinate transformation. The drag model used is a polynomial fit of Cd vs Mach number for an Atlas V launch vehicle, while a simple spherical two-body central gravity field is assumed. The Earth is assumed to be non-rotating. 

Currently only supports two-stage launch vehicles. The first stage will execute a gravity turn trajectory whereby the thrust vector is aligned to the velocity vector until stage one burnout. After this, the upper stage closed loop ascent guidance, based on powered explicit guidance, takes over. 

The PEG routine used in this is based on the Unified Powered Flight Guidance routine used on the Space Shuttle, and is implemented exactly as described in "Space Shuttle GNC Document 24, Unified Powered Flight Guidance", by Brand, Brown, and Higgins. This routine requires a target orbit radius and velocity magnitude, as well as unit vector normal to the orbit plane (provided through desired inclination and RAAN), and computes the steering angles in pitch and yaw in order to achieve the required burnout conditions. The routine is called initially immediately after the end of stage 1 separation, and recycled repeatedly until convergence of the required velocity-to-go, as well as the time-to-go until engine cutoff. After this, the routine is called upon each integration loop and steering commands are issued to the vehicle. 
