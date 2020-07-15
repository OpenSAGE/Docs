SlowDeathBehavior Module
========================

ProbabilityModifier
-------------------

``ProbabilityModifier`` is used to choose between several ``SlowDeathBehavior``s
when more than one is used on the same object.

DeathFlags
----------

These flags are added to both model condition and object status.

Flinging
--------

When an object dies, and ``FlingForce`` is greater than zero, a force is added
to the object's ``PhysicsBehavior`` based on the following properties:

* ``FlingForce``
* ``FlingForceVariance``
* ``FlingPitch``
* ``FlingPitchVariance``

Sinking
-------

When an object dies, it first waits until ``SinkDelay``, plus or minus a random
duration based on ``SinkDelayVariance``, has elapsed.

Then it starts to sink at the rate specified by ``SinkRate``.

Fading
------

When an object dies, it first waits until ``FadeDelay`` has elapsed.

Then it starts to fade out at the rate specified by ``FadeTime``.

Decaying
--------

After ``DecayBeginTime``, the ``DECAY`` model condition flag is added to the object.

Destruction
-----------

When an object dies, it enters the ``INITIAL`` phase of destruction.

A destruction duration is calculated, based on ``DestructionDelay`` plus or
minus a random duration based on ``DestructionDelayVariance``.

* ``INITIAL``: Any ``FX``, ``OCL``, or ``Weapon`` specified for the ``INITIAL``
  phase will be triggered. After the first half of the calculated destruction
  delay has elapsed, the phase changes to ``MIDPOINT``.

* ``MIDPOINT``: Any ``FX``, ``OCL``, or ``Weapon`` specified for the
  ``MIDPOINT`` phase will be triggered. After the second half of the calculated
  destruction delay has elapsed, the phase changes to ``FINAL``.

* ``FINAL``: Any ``FX``, ``OCL``, or ``Weapon`` specified for the ``FINAL``
  phase will be triggered. Then the object will be destroyed.