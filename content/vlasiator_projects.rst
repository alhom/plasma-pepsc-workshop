Vlasiator Project Structure
===========================

Why we teach this lesson
------------------------

Here we go through some available Vlasiator projects and how those are employed to set up different environments.


Intended learning outcomes
--------------------------

The user has an overview of main types of projects included in Vlasiator and can configure them.


"Project"
---------

Let's look at some sections of a Vlasiator configuration file:

.. code-block::

    ParticlePopulations = proton

    project = Flowthrough
    propagate_field = 1
    propagate_vlasov_acceleration = 1
    propagate_vlasov_translation = 1
    dynamic_timestep = 1

    [proton_properties]
    ...
    [AMR]
    ...
    [gridbuilder]
    ...
    ...

    [Flowthrough]
    Bx = 1.0e-9
    By = 1.0e-9
    Bz = 1.0e-9

    [proton_Flowthrough]
    T = 1.0e5
    rho = 1.0e6
    VX0 = 1e5
    VY0 = 0
    VZ0 = 0


Here, we have specified ``project = Flowthrough``, and some input under ``[Flowthrough]`` and ``proton_Flowthrough``. These are, as the name suggests, the project inputs for the configuration. Let's see what these are made of and fetch the ``./vlasiator --help`` outputs.

.. code-block::
    
    [Flowthrough]
    Bx = 1.0e-9
    By = 1.0e-9
    Bz = 1.0e-9
    
    --Flowthrough.Bx arg (=0)  ... Magnetic field x component (T)
    --Flowthrough.By arg (=0)  ... Magnetic field y component (T)
    --Flowthrough.Bz arg (=0)  ... Magnetic field z component (T)
    

That's pretty self-explanatory! notably, the ``arg (=0)`` shows the syntax and default value, and the unit ``(T)`` is given at the end. Similarly:

.. code-block::

    [proton_Flowthrough]
    T = 1.0e5
    rho = 1.0e6
    VX0 = 1e5
    VY0 = 0
    VZ0 = 0

    --<population>_Flowthrough.rho arg (=0)      Number density (m^-3)
    --<population>_Flowthrough.T arg (=0)        Temperature (K)
    --<population>_Flowthrough.VX0 arg (=0)      Initial bulk velocity in x-direction
    --<population>_Flowthrough.VY0 arg (=0)      Initial bulk velocity in y-direction
    --<population>_Flowthrough.VZ0 arg (=0)      Initial bulk velocity in z-direction
   
These describe the initial population parameters, for each ion population. Inflow boundaries are described elsewhere (see ``[proton_Maxwellian]``!). There are few unused options provided by the project:

.. code-block:: 

      --Flowthrough.densityModel arg (=Maxwellian)  Plasma density model, 'Maxwellian' or 'SheetMaxwellian'
      --Flowthrough.densityWidth arg (=60000000)    Width of signal around origin
      --<population>_Flowthrough.rhoBase arg (=0)   Background number density (m^-3)

These are a bit more interesting! ``densityModel`` defines the (spatial) density profile (and there are unlisted options, such as ``Square``) and `densityWidth` gives a scale width for the signal. ``Maxwellian`` is a bit confusingly a spatially-constant density, and then ``rhoBase`` sets a background density besides the density signal.

Notably, these all are related to the initial conditions - system boundaries are defined by their own classes and configuration headers (like ``[proton_Maxwellian]``).

So what are Projects?
^^^^^^^^^^^^^^^^^^^^^

Vlasiator *projects*  are a modular component for setting up Vlasiator simulations. These enable separating e.g. the initialization logic of simulation from the actual solvers, with their own input parameter sets. These can be found in the Vlasiator source code, under the folder `projects <https://github.com/fmihpc/vlasiator/tree/master/projects>`_. Besides initialization, some hooks are provided in the main solver for a project to perform specialized tasks during the simulation.

For example, the Flowthrough project source folder looks like this:

.. code-block:: bash

    /vlasiator/projects/Flowthrough$ ls
    Flowthrough_amr_test_20190611_YPK.cfg
    Flowthrough.cfg
    Flowthrough.cpp
    Flowthrough.h
    run_amr_test_20190611_YPK.sh
    sw1_amr_test_20190611_YPK.dat
    sw1.dat

There are the project code definitions (``Flowthrough.h`` and ``Flowthrough.cpp``) and and example configuration (``Flowthrough.cfg``, with an associated ``sw1.dat``), and some historical tests preserved for posteriority.




Other practical aspects
-----------------------




Interesting questions you might get
-----------------------------------



Typical pitfalls
----------------
