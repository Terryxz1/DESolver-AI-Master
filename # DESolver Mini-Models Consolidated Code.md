## Consolidated Mini-Models Code 
Below is the code for individual mini-models created by Tom Coleman to understand the DESolver XML structure and mathematical capabilities. 


Adaptation

Created : 25-Aug-96
Last Modified : 30-Nov-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

A disturbance is initially propagated from
input to output.  As time passes, the output
gradually returns to its previous value.

Adaptation is defined by

   dA/dt = K * O
   O = I - A

K is the rate constant of the time delay.  The
time constant (Tau) of the delay is the
reciprocal of the rate constant.

   Tau = 1 / K

For a step change in input, when time = Tau,
O = 0.63 * I.

The half-time of the step response is the time
required for the output go one-half way to its
final value.

   t 1/2 = 0.69 * Tau

<?xml version = '1.0' ?>

<model>

<title><basic> Adaptation </basic></title>

<structure>
<name> Model </name>

<variables> =====================================

K is the adaptation rate constant.

<parm>
  <name> K </name>
  <val> 0.2 </val>
</parm>

A step function of amplitude Amp hits when X is
equal to At.  Note that the variable X is
DESolver's internal variable representing the
model's independent variable.

<parm>
  <name> Amp </name>
  <val> 1.0 </val>
</parm>

<parm>
  <name> At </name>
  <val> 4.0 </val>
</parm>

<var>
  <name> Tau </name>
</var>

<var>
  <name> t1/2 </name>
</var>

<var>
  <name> I </name>
  <val> 0 </val>
</var>

<var>
  <name> O </name>
</var>

</variables>

<equations> =====================================

<diffeq>
  <name> A </name>
  <integralname> A </integralname>
  <initialval> 0.0 </initialval>
  <dervname> dA/dt </dervname>
</diffeq>

Note that no limit has been specified for
integration error.  This could be a problem
if the adaptation time is much shorter than
the display interval.

</equations>

<definitions> ===================================

<block><name> Parms </name> =====================

<def>
  <name> Tau </name>
  <val> INVERT K </val>
</def>

<def>
  <name> t1/2 </name>
  <val> 0.69 * Tau </val>
</def>

</block>

<block><name> Dervs </name> =====================

<conditional>
  <name> I </name>
  <test> System.X GE At </test>
  <true> Amp </true>
</conditional>

<def>
  <name> O </name>
  <val> I - A </val>
</def>

<def>
  <name> dA/dt </name>
  <val> K * O </val>
</def>

</block>

</definitions>
</structure>

<math> ==========================================

  <parms> Model.Parms </parms>
  <dervs> Model.Dervs </dervs>

</math>

<control> =======================================

<gofor>
  <solutionint> 10 </solutionint>
  <displayint> 0.5 </displayint>
</gofor>

<gofor>
  <solutionint> 20 </solutionint>
  <displayint> 1.0 </displayint>
</gofor>

</control>

<display> =======================================
<panel>

<name> Adaptation </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<showvalue>
   <row> 2 </row><col> 1 </col>
   <name> I </name>
   <format><decimal> 1 </decimal></format>
   <label> Input </label>
</showvalue>

<showvalue>
   <row> 3 </row><col> 1 </col>
   <name> O </name>
   <format><decimal> 1 </decimal></format>
   <label> Output </label>
</showvalue>

<showvalue>
   <row> 4 </row><col> 1 </col>
   <name> A </name>
   <format><decimal> 1 </decimal></format>
   <label> Adaptation </label>
</showvalue>

<showvalue>
   <row> 5 </row><col> 1 </col>
   <name> dA/dt </name>
   <format><decimal> 2 </decimal></format>
   <label> dA/dt </label>
</showvalue>

Input

<groupbox>
   <row> 6.6 </row><col> 1 </col>
   <high> 4.8 </high><wide> 26 </wide>
   <title> Step Input </title>

<repeatlist>
  <name> Amp </name>
  <repeat><reps> 5 </reps><stepsize> 1 </stepsize></repeat>
</repeatlist>

<slidebar>
  <row> 1.6 </row><col> 1 </col>
  <name> Amp </name>
  <listname> Amp </listname>
  <label> Size </label>
</slidebar>

<repeatlist>
  <name> At </name>
  <repeat><reps> 20 </reps><stepsize> 1 </stepsize></repeat>
</repeatlist>

<slidebar>
  <row> 3.2 </row><col> 1 </col>
  <name> At </name>
  <listname> At </listname>
  <label> At </label>
</slidebar>

</groupbox>

Rate Constant

<groupbox>
   <row> 11.6 </row><col> 1 </col>
   <high> 5.4 </high><wide> 26 </wide>
   <title> Rate Constant </title>

<repeatlist>
  <name> K </name>
  <firstval> 0.01 </firstval>
  <repeat><reps> 19 </reps><stepsize> 0.01 </stepsize></repeat>
  <repeat><reps> 8 </reps><stepsize> 0.1 </stepsize></repeat>
</repeatlist>

<slidebar>
  <row> 1.6 </row><col> 1 </col>
  <name> K </name>
  <listname> K </listname>
  <label> Size </label>
</slidebar>

<showvalue>
   <row> 3.0 </row><col> 1 </col>
   <name> Tau </name>
   <format><decimal> 1 </decimal></format>
   <label> Time Constant </label>
</showvalue>

<showvalue>
   <row> 4.0 </row><col> 1 </col>
   <name> t1/2 </name>
   <format><decimal> 1 </decimal></format>
   <label> Half-Life </label>
</showvalue>

</groupbox>

Right-hand Side ================================

<showvalue>
   <row> 0.5 </row><col> 47 </col>
   <name> System.X </name>
   <format><decimal> 1 </decimal></format>
   <label> Time </label>
</showvalue>

<showgraph>
  <row> 2.5 </row><col> 28 </col>
  <high> 10 </high><wide> 32 </wide>
  <leftmargin> 6 </leftmargin>
  <xaxis>
     <name> System.X </name>
     <label> Time </label>
     <scale><min> 0 </min><max> 20 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> O </name>
      <label> O </label>
      <linecolor> BLUE </linecolor>
    </yvar>
    <yvar>
      <name> I </name>
      <label> I </label>
      <linecolor> RED </linecolor>
    </yvar>
    <yvar>
      <name> A </name>
      <label> A </label>
      <linecolor> MAGENTA </linecolor>
    </yvar>
    <scale><min> 0 </min><max> 1 </max></scale>
  </yaxis>
</showgraph>

<showgraph>
  <row> 13.4 </row><col> 28 </col>
  <high> 6 </high><wide> 32 </wide>
  <leftmargin> 6 </leftmargin>
  <xaxis>
     <name> System.X </name>
     <label> Time </label>
     <scale><min> 0 </min><max> 20 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> dA/dt </name>
      <label> dA/dt </label>
    </yvar>
    <scale><min> -0.1 </min><max> 0.1 </max><inc> 0.1 </inc></scale>
  </yaxis>
</showgraph>

<infobutton>
  <row> 0.5 </row><col> 13 </col>
  <label> The Equation </label>
  <line> The adaptation equation is </line>
  <line>  </line>
  <line> dA / dt = K * O </line>
  <line> O = I - A </line>
  <line>  </line>
  <line> where I is input, O is output, </line>
  <line> and A is adaptation. </line>
</infobutton>

<infobutton>
  <row> 0.5 </row><col> 29 </col>
  <label> Rate Constant </label>
  <line> K is the rate constant in the </line>
  <line> adaptation equation </line>
  <line>  </line>
  <line> dA / dt = K * O </line>
  <line> O = I - A </line>
  <line>  </line>
  <line> The time constant (Tau) of the </line>
  <line> adaptation is the reciprocal </line>
  <line> of the rate constant. </line>
  <line>  </line>
  <line> Tau = 1 / K </line>
  <line>  </line>
  <line> For a step change in input, </line>
  <line> when time is one time constant </line>
  <line> beyond the step, the output is </line>
  <line> 63% adapted. </line>
  <line>  </line>
  <line> The half-time of the step </line>
  <line> response is the time required </line>
  <line> for the output to go one-half </line>
  <line> of the way back to its original </line>
  <line> value. </line>
  <line>  </line>
  <line> t 1/2 = 0.69 * Tau </line>
</infobutton>

</panel>
</display>

</model>

End
Bouncing Ball Model

Created : 11-Aug-96
Last Modified : 30-Nov-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

The ball is released above a hard surface.  It
accelerates downward until it strikes the surface.
Then, velocity reverses as a function of its
elasticity and it bounces back up again.

Distance is the integral of velocity
over time while velocity is the integral
of acceleration over time.

This model is two-dimensional.  Acceleration,
velocity and distance are described using
vertical and horizontal components.

Horizontal acceleration is zero.  Horizontal
velocity is the integral of horizontal
acceleration, and it should be constant.  Horizontal
distance is the integral of horizontal velocity.
Thus, we have two differential equations in the
horizontal direction.

Vertical acceleration is produced by gravity and
is constant.  Vertical velocity is the integral
of vertical acceleration while vertical distance
is the integral of vertical velocity.  Thus, we
have two differential equations in the vertical
direction.

The ball is elastic.  It reverses velocity
when it hits the ground.

<?xml version = '1.0' ?>

<model>

<title><basic> Bouncing Ball </basic></title>

<structure>
<name> Model </name>

<variables> ---------------------------------------

Horizontal acceleration is 0 while vertical
(upward) acceleration due to gravity is
-9.8 m/sec^2 at the earth's surface.

<constant>
  <name> HorzAcc </name>
  <val> 0.0 </val>
</constant>

<constant>
  <name> VertAcc </name>
  <val> -9.8 </val>
</constant>

<parm>
  <name> Elasticity </name>
  <val> 0.8 </val>
</parm>

</variables>

<equations> ----------------------------------

Define velocity before distance, since the
definition of velocity is referred to in the
definition of distance.

<diffeq>
  <name> HorzVel </name>
  <integralname> HorzVel </integralname>
  <initialval> 1.0 </initialval>
  <dervname> dHV/dt </dervname>
  <errorlim> 0.1 </errorlim>
</diffeq>

<diffeq>
  <name> HorzDis </name>
  <integralname> HorzDis </integralname>
  <initialval> 0.0 </initialval>
  <dervname> dHD/dt </dervname>
  <errorlim> 0.1 </errorlim>
</diffeq>

<diffeq>
  <name> VertVel </name>
  <integralname> VertVel </integralname>
  <initialval> 0.0 </initialval>
  <dervname> dVV/dt </dervname>
  <errorlim> 0.01 </errorlim>
</diffeq>

<diffeq>
  <name> VertDis </name>
  <integralname> VertDis </integralname>
  <initialval> 10.0 </initialval>
  <dervname> dVD/dt </dervname>
  <errorlim> 0.01 </errorlim>
</diffeq>

</equations>

<definitions> -----------------------------------

This is strange, but the derivative code is mainly
deciding when to reverse the vertical velocity, that
is, when to make the ball bounce.

<block><name> Dervs </name> ====================

<def>
  <name> dHV/dt </name>
  <val> HorzAcc </val>
</def>

<def>
  <name> dHD/dt </name>
  <val> HorzVel </val>
</def>

<def>
  <name> dVV/dt </name>
  <val> VertAcc </val>
</def>

<def>
  <name> dVD/dt </name>
  <val> VertVel </val>
</def>

</block>

<block><name> Wrapup </name> ====================

<if><test> VertDis LE 0 </test>
<true>

  <def>
    <name> VertDis </name>
    <val> 0.0 </val>
  </def>

  <def>
    <name> dVD/dt </name>
    <val> ABS ( VertVel ) * Elasticity </val>
  </def>

  <def>
    <name> VertVel </name>
    <val> dVD/dt </val>
  </def>

</true>
</if>

</block>
</definitions>
</structure>

<math>
  <dervs> Model.Dervs </dervs>
  <wrapup> Model.Wrapup </wrapup>
</math>

<control> ========================================

<gofor>
  <solutionint> 1 </solutionint>
  <displayint> 0.1 </displayint>
</gofor>

<goto>
  <solutionint> 10 </solutionint>
  <displayint> 0.1 </displayint>
  <menuitem> Go To 10 </menuitem>
</goto>

</control>

<display> =========================================

<panel><name> Bouncing Ball </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<repeatlist>
  <name> elastics </name>
  <repeat><reps> 20 </reps><stepsize> 0.1 </stepsize></repeat>
</repeatlist>

<slidebar>
  <row> 2.4 </row><col> 1 </col>
  <name> Elasticity </name>
  <listname> elastics </listname>
  <label> Elasticity </label>
</slidebar>

Vertical

<groupbox>
   <row> 4.0 </row><col> 1 </col>
   <high> 5.0 </high><wide> 24 </wide>
   <title> Vertical </title>

<showvalue>
  <row> 1.4 </row><col> 1 </col>
  <name> VertDis </name>
  <format><decimal> 1 </decimal></format>
  <label> Distance </label>
</showvalue>

<showvalue>
  <row> 2.4 </row><col> 1 </col>
  <name> VertVel </name>
  <format><decimal> 1 </decimal></format>
  <label> Velocity </label>
</showvalue>

<showvalue>
  <row> 3.4 </row><col> 1 </col>
  <name> VertAcc </name>
  <format><decimal> 1 </decimal></format>
  <label> Acceleration </label>
</showvalue>

</groupbox>

Horizontal

<groupbox>
   <row> 10.0 </row><col> 1 </col>
   <high> 5.0 </high><wide> 24 </wide>
   <title> Horizontal </title>

<showvalue>
  <row> 1.4 </row><col> 1 </col>
  <name> HorzDis </name>
  <format><decimal> 1 </decimal></format>
  <label> Distance </label>
</showvalue>

<showvalue>
  <row> 2.4 </row><col> 1 </col>
  <name> HorzVel </name>
  <format><decimal> 1 </decimal></format>
  <label> Velocity </label>
</showvalue>

<showvalue>
  <row> 3.4 </row><col> 1 </col>
  <name> HorzAcc </name>
  <format><decimal> 1 </decimal></format>
  <label> Acceleration </label>
</showvalue>

</groupbox>

Units

<infobutton>
  <row> 16 </row><col> 1 </col>
  <label> Units </label>
  <line> Time - Sec </line>
  <line> Distance - M </line>
  <line> Velocity - M / Sec </line>
  <line> Acceleration - M / Sec^2 </line>
</infobutton>

Right-hand Side --------------------------------

<showvalue>
   <row> 0.5 </row><col> 27 </col>
   <name> System.X </name>
   <format><decimal> 1 </decimal></format>
   <label> Time </label>
</showvalue>

<showgraph>
  <row> 2.0 </row><col> 27 </col>
  <high> 12 </high><wide> 30 </wide>
  <leftmargin> 5 </leftmargin>
  <xaxis>
    <name> HorzDis </name>
    <label> X </label>
    <scale><min> 0 </min><max> 10 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> VertDis </name>
      <label> Y </label>
      <linecolor> RED </linecolor>
    </yvar>
    <scale><min> 0 </min><max> 10 </max></scale>
  </yaxis>
</showgraph>

<showgraph>
  <row>14.9 </row><col> 27 </col>
  <high> 7 </high><wide> 30 </wide>
  <leftmargin> 5 </leftmargin>
  <xaxis>
    <name> HorzDis </name>
    <label> X </label>
    <scale><min> 0 </min><max> 10 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> VertVel </name>
      <label> dY/dt </label>
      <linecolor> BLUE </linecolor>
    </yvar>
    <scale><min> -15 </min><max> 15 </max><inc> 15 </inc></scale>
  </yaxis>
</showgraph>

</panel>
</display>

</model>

End  Cardiac Output and Venous Return

Created : 12-Aug-96
Last Modified : 30-Nov-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

On the average, cardiac output and venous return are
equal.  Each is determined by a unique set of factors.

Cardiac output (CO) is determined by the right atrial
pressure (RAP), heart strength, and shape of the Frank-
Starling curve.

   CO = f (RAP) * Heart Strength                  ( 1 )

Venous return is determined by right atrial pressure,
mean circulatory filling pressure and the resistance
to venous return.

   VR = (MCFP - RAP) * RVR                        ( 2 )

We assume that cardiac output (CO) is equal to venous
return (VR).

   CO = VR                                        ( 3 )   

Equations (1), (2) and (3) can be combined to form an
implicit algebraic equation.

   CO = f (MCFP - CO / RVR) * Hrt Strngth

This equation has the form of Y = f(Y).  It can be
solved numerically using

   YEnd = f(YStart)

with a root denoted by

   | YEnd - YStart | LE ErrLimit

<?xml version = '1.0' ?>

<model>

<title><basic> Windkessel Model </basic></title>

<structure>
<name> Model </name>

<variables> ====================================

<parm>
  <name> RVR </name>
  <val> 0.0013 </val>
</parm>

<parm>
  <name> MCFP </name>
  <val> 7.0 </val>
</parm>

<parm>
  <name> HeartStrength </name>
  <val> 1.0 </val>
</parm>

<var><name> DelP </name></var>
<var><name> RAP </name></var>

</variables>

<equations> ====================================

<impliciteq>
  <name> CO </name>
  <startname> CO </startname>
  <initialval> 5400.0 </initialval>
  <endname> COEnd </endname>
  <errorlim> 10.0 </errorlim>
</impliciteq>

</equations>

<functions> ====================================

<curve>
  <name> Frank-Starling </name>
  <point><x> -4 </x><y>     0 </y><slope> 0 </slope></point>
  <point><x>  0 </x><y>  5400 </y><slope> 1500 </slope></point>
  <point><x> 10 </x><y> 12500 </y><slope> 0 </slope></point>
</curve>

</functions>

<definitions> =======================================================

<block><name> Dervs </name> =========================================

<implicitmath><name> CO </name>

<def>
  <name> DelP </name>
  <val> CO * RVR </val>
</def>

<def>
  <name> RAP </name>
  <val> MCFP - DelP </val>
</def>

<call> Pump </call>

</implicitmath>

</block>

<block><name> Pump </name>

<def>
  <name> COEnd </name>
  <val> Frank-Starling [ RAP ] * HeartStrength </val>
</def>

</block>

</definitions>
</structure>

<math>
  <dervs> Model.Dervs </dervs>
</math>

There is no control section because this is
purely an algebraic relationship.  The
independent variable X is not involved.

<display> =======================================
<panel>

<name> Frank-Starling Calculation </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<editbox>
  <row> 3 </row><col> 1 </col>
  <name> MCFP </name>
  <label> MCFP </label>
  <min> -5 </min>
  <max> 30 </max>
</editbox>

<showvalue>
   <row> 4.6 </row><col> 1 </col>
   <name> CO </name>
   <format> <integer/> </format>
   <label> CO </label>
</showvalue>

<showvalue>
   <row> 5.6 </row><col> 1 </col>
   <name> DelP </name>
   <format><decimal> 1 </decimal></format>
   <label> Pressure Gradient </label>
</showvalue>

<showvalue>
   <row> 6.6 </row><col> 1 </col>
   <name> RAP </name>
   <format><decimal> 1 </decimal></format>
   <label> RAP </label>
</showvalue>

<showvalue>
   <row> 7.6 </row><col> 1 </col>
   <name> RVR </name>
   <format><decimal> 4 </decimal></format>
   <label> RVR </label>
</showvalue>

<repeatlist>
  <name> HeartStrength </name>
  <repeat><reps> 20 </reps><stepsize>  0.1 </stepsize></repeat>
</repeatlist>

<slidebar>
  <row> 9.0 </row><col> 1.0 </col>
  <name> HeartStrength </name>
  <listname> HeartStrength </listname>
  <label> Heart Strength </label>
</slidebar>

===============================================

<showmap>
  <row> 2.0 </row><col> 30.0 </col>
  <high> 9 </high><wide> 28 </wide>
  <leftmargin> 8 </leftmargin>

  <xaxis>
    <name> RAP </name>
    <label> RAP </label>
    <scale><min> -5 </min><max> 20 </max><inc> 5 </inc></scale>
  </xaxis>

  <yaxis>
    <name> COEnd </name>
    <label> CO </label>
    <scale><min> 0 </min><max> 15000 </max><inc> 5000 </inc></scale>
  </yaxis>

  <blockname> Pump </blockname>
</showmap>

</panel>
</display>

</model>

EndDistance

Created : 10-Aug-96
Last Modified : 30-Nov-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

We're shooting the shotput.  The idea here
is to find the angle (from horizontal) that
makes the shotput go the greatest distance
-- at a constant initial velocity.

Distance is the integral of velocity
over time while velocity is the integral
of acceleration over time.

This model is two-dimensional.  Acceleration,
velocity and distance are described using
vertical and horizontal components.

<?xml version = '1.0' ?>

<model>

<title><basic> Distance </basic></title>

<structure>
<name> Model </name>

<variables> =======================================

We define the initial angle and velocity here
using fixed parameters.  This information is
used at the start of the solution to start the
integrals.

<parm>
  <name> InitAngle </name>
  <val> 70 </val>
</parm>

<parm>
  <name> InitVelocity </name>
  <val> 10 </val>
</parm>

The horizontal acceleration is zero.  The
vertical acceleration is gravity, or -9.8
m/sec^2 at the earth's surface.

<constant>
  <name> HorzAcc </name>
  <val> 0 </val>
</constant>

<constant>
  <name> VertAcc </name>
  <val> -9.8 </val>
</constant>

<var>
  <name> Radians </name>
</var>

<var>
  <name> MaxAltitude </name>
  <val> 0 </val>
</var>

</variables>

<equations> =======================================

The horizontal velocity is the integral over
time of the horizontal acceleration.  We'll
use some context code below to set the initial
velocity.

Horizontal distance is the integral over time of
the horizontal velocity.

<diffeq>
  <name> HorzVel </name>
  <integralname> HorzVel </integralname>
  <initialval> 0.0 </initialval>
  <dervname> dHorzVel/dt </dervname>
  <errorlim> 0.1 </errorlim>
</diffeq>

<diffeq>
  <name> HorzDis </name>
  <integralname> HorzDis </integralname>
  <initialval> 0.0 </initialval>
  <dervname> dHorzDis/dt </dervname>
  <errorlim> 0.1 </errorlim>
</diffeq>

The vertical velocity is the integral over
time of the vertical acceleration.  We'll
use some context code below to set the initial
velocity.

Vertical distance is the integral over time of
the vertical velocity.

<diffeq>
  <name> VertVel </name>
  <integralname> VertVel </integralname>
  <initialval> 0.0 </initialval>
  <dervname> dVertVel/dt </dervname>
  <errorlim> 0.1 </errorlim>
</diffeq>

<diffeq>
  <name> VertDis </name>
  <integralname> VertDis </integralname>
  <initialval> 0.0 </initialval>
  <dervname> dVertDis/dt </dervname>
  <errorlim> 0.1 </errorlim>
</diffeq>

</equations>

<definitions> =====================================

<block><name> Parms </name> =======================

The following code is executed when a parameter
value is changed.

<def>
  <name> Radians </name>
  <val> InitAngle * System.Pi / 180 </val>
</def>

<def>
  <name> VertVel </name>
  <val> InitVelocity * SIN Radians </val>
</def>

<def>
  <name> HorzVel </name>
  <val> InitVelocity * COS Radians </val>
</def>

</block>

<block><name> Dervs </name> =======================

<def>
  <name> dHorzVel/dt </name>
  <val> HorzAcc </val>
</def>

<def>
  <name> dHorzDis/dt </name>
  <val> HorzVel </val>
</def>

<def>
  <name> dVertVel/dt </name>
  <val> VertAcc </val>
</def>

<def>
  <name> dVertDis/dt </name>
  <val> VertVel </val>
</def>

<if>
  <test> VertDis GT MaxAltitude </test>
  <true>
    <def>
      <name> MaxAltitude </name>
      <val> VertDis </val>
    </def>
  </true>
</if>

</block>

<block><name> Wrapup </name> ======================

Stop the solution when height falls to below 0.

<if>
  <test> VertDis LT 0 </test>
  <true>
    <def>
     <name> VertDis </name>
      <val> 0 </val>
    </def>
    <stop/>
  </true>
</if>

</block>

</definitions>
</structure>

<math>

  <parms> Model.Parms </parms>
  <dervs> Model.Dervs </dervs>
  <wrapup> Model.Wrapup </wrapup>

</math>

<control> ========================================

<gofor>
  <solutionint> 5 </solutionint>
  <displayint> 0.05 </displayint>
</gofor>

</control>

<display> =========================================
<panel>

<name> Distance (Shotput) </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<showgraph>
  <row> 2 </row><col> 32 </col>
  <high> 14 </high><wide> 30 </wide>
  <leftmargin> 7 </leftmargin>
  <xaxis>
    <name> HorzDis </name>
    <label> Distance </label>
    <scale><min> 0 </min><max> 10 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> VertDis </name>
      <label> Height </label>
    </yvar>
    <scale><min> 0 </min><max> 10 </max></scale>
  </yaxis>
</showgraph>

<groupbox>
   <row> 2 </row><col> 1 </col>
   <high> 6.4 </high><wide> 30 </wide>
   <title> Initial Values </title>

<repeatlist>
  <name> InitAngle </name>
  <repeat>
    <reps> 50 </reps>
    <stepsize> 2 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 1.5 </row><col> 1 </col>
  <name> InitAngle </name>
  <listname> InitAngle </listname>
  <label> Angle </label>
</slidebar>

<showvalue>
   <row> 3.0 </row><col> 1 </col>
   <name> Radians </name>
   <format><decimal> 2 </decimal></format>
   <label> Radians </label>
</showvalue>

<repeatlist>
  <name> InitVelocity </name>
  <repeat>
    <reps> 20 </reps>
    <stepsize> 1 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 4.5 </row><col> 1 </col>
  <name> InitVelocity </name>
  <listname> InitVelocity </listname>
  <label> Velocity </label>
</slidebar>

</groupbox>

<showvalue>
   <row> 10.4 </row><col> 1 </col>
   <name> System.X </name>
   <format><decimal> 1 </decimal></format>
   <label> Time </label>
</showvalue>

<showvalue>
   <row> 11.8 </row><col> 1 </col>
   <name> VertDis </name>
   <format><decimal> 1 </decimal></format>
   <label> Height </label>
</showvalue>

<showvalue>
   <row> 12.8 </row><col> 1 </col>
   <name> VertVel </name>
   <format><decimal> 1 </decimal></format>
   <label> Vertical Velocity </label>
</showvalue>

<showvalue>
   <row> 13.8 </row><col> 1 </col>
   <name> VertAcc </name>
   <format><decimal> 1 </decimal></format>
   <label> Vertical Acceleration </label>
</showvalue>

<showvalue>
   <row> 15.2 </row><col> 1 </col>
   <name> HorzDis </name>
   <format><decimal> 1 </decimal></format>
   <label> Distance </label>
</showvalue>

<showvalue>
   <row> 16.2 </row><col> 1 </col>
   <name> HorzVel </name>
   <format><decimal> 1 </decimal></format>
   <label> Horizontal Velocity </label>
</showvalue>

<showvalue>
   <row> 17.2 </row><col> 1 </col>
   <name> HorzAcc </name>
   <format><decimal> 1 </decimal></format>
   <label> Horizontal Acceleration </label>
</showvalue>

<showvalue>
   <row> 18.6 </row><col> 1 </col>
   <name> MaxAltitude </name>
   <format><decimal> 1 </decimal></format>
   <label> Maximum Altitude </label>
</showvalue>

</panel>
</display>

</model>

EndDrug Dosing

Created : 24-Aug-96
Last Modified : 01-Dec-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

Watch the concentration of drug build
up over time with repeated dosing.

A medicine is taken N times a day. The
medicine is continuously degraded in
proportion to the total amount in the
body.  The concentration in the body
is a time-dependent function of both
dosing and degradation.

The independent variable is time in days.

<?xml version = '1.0' ?>

<model>

<title><basic> Drug Dosing </basic></title>

<structure>
<name> Model </name>

<variables> ======================================

<parm>
  <name> K </name>
  <val> 2.0 </val>
</parm>

<parm>
  <name> Dose </name>
  <val> 5 </val>
</parm>

<parm>
  <name> TimesDay </name>
  <val> 4 </val>
</parm>

<var>
  <name> LastDose </name>
  <val> 0 </val>
</var>

<var>
  <name> Peak </name>
  <val> 0 </val>
</var>

<var>
  <name> Interval </name>
</var>

</variables>

<equations> ====================================

<diffeq>
  <name> Q </name>
  <integralname> Q </integralname>
  <initialval> 0.0 </initialval>
  <dervname> dQ/dt </dervname>
</diffeq>

</equations>

<definitions> ======================================

<block><name> Parms </name> ========================

<def>
  <name> Interval </name>
  <val> INVERT TimesDay </val>
</def>

</block>

<block><name> Dervs </name> ========================

<def>
  <name> dQ/dt </name>
  <val> - K * Q </val>
</def>

</block>

<block><name> Wrapup </name> =======================

<if>
  <test> System.X GE ( LastDose + Interval ) </test>
  <true>
    <def>
      <name> LastDose </name>
      <val> LastDose + Interval </val>
    </def>
    <def>
      <name> Q </name>
      <val> Q + Dose </val>
    </def>
  </true>
</if>

<if>
  <test> Q GT Peak </test>
  <true>
    <def>
      <name> Peak </name>
      <val> Q </val>
    </def>
  </true>
</if>

</block>

</definitions>
</structure>

<math> ==========================================

  <parms> Model.Parms </parms>
  <dervs> Model.Dervs </dervs>
  <wrapup> Model.Wrapup </wrapup>

</math>

<control> ========================================


<gofor>
  <solutionint> 2 </solutionint>
  <displayint> 0.05 </displayint>
</gofor>


</control>

<display> ========================================
<panel>

<name> Drug Dosing </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<showvalue>
   <row> 2 </row><col> 1 </col>
   <name> Q </name>
   <format> <integer/> </format>
   <label> Conc </label>
</showvalue>

<showvalue>
   <row> 3 </row><col> 3 </col>
   <name> dQ/dt </name>
   <format> <integer/> </format>
   <label> Change </label>
</showvalue>

Rx

<groupbox>
   <row> 5 </row><col> 1 </col>
   <high> 4.6 </high><wide> 28 </wide>
   <title> Rx </title>

<repeatlist>
  <name> Dose </name>
  <repeat>
    <reps> 20 </reps>
    <stepsize> 1 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 1.6 </row><col> 1 </col>
  <name> Dose </name>
  <listname> Dose </listname>
  <label> Dose </label>
</slidebar>

<repeatlist>
  <name> TimesDay </name>
  <repeat>
    <reps> 10 </reps>
    <stepsize> 1 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 3.0 </row><col> 1 </col>
  <name> TimesDay </name>
  <listname> TimesDay </listname>
  <label> Times / Day </label>
</slidebar>

</groupbox>

Clearance

<groupbox>
   <row> 10 </row><col> 1 </col>
   <high> 3.2 </high><wide> 28 </wide>
   <title> Clearance </title>

<maplist>
  <name> K </name>
  <map><val> 0.0 </val><img> None </img></map>
  <map><val> 1.0 </val><img> Poor </img></map>
  <map><val> 2.0 </val><img> Normal </img></map>
  <map><val> 4.0 </val><img> Good </img></map>
  <map><val> 10.0 </val><img> Excellent </img></map>
</maplist>

<slidebar>
  <row> 1.6 </row><col> 1 </col>
  <name> K </name>
  <listname> K </listname>
  <label> Rate </label>
</slidebar>

</groupbox>

Greatest Concentration

<showvalue>
   <row> 14 </row><col> 1 </col>
   <name> Peak </name>
   <format> <integer/> </format>
   <label> Greatest Concentration </label>
</showvalue>

Right-hand Side =================================

<showclock>
   <row> 0.5 </row><col> 31 </col>
   <name> System.X </name>
   <timebase> DAY </timebase>
   <days/>
</showclock>

<showgraph>
  <row> 2.5 </row><col> 31 </col>
  <high> 14 </high><wide> 32 </wide>
  <leftmargin> 5 </leftmargin>
  <xaxis>
    <name> System.X </name>
    <label> Days </label>
    <scale><min> 0 </min><max> 2 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> Q </name>
      <label> Conc </label>
      <linecolor> RED </linecolor>
    </yvar>
    <scale><min> 0 </min><max> 5 </max></scale>
  </yaxis>
</showgraph>

</panel>
</display>

</model>

End
Enzyme Kinetics

Created : 22-Aug-96
Last Modified : 01-Dec-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

Substrate (S) and enzyme (E) combine to form the
enzyme/substrate complex (ES).  The complex then
dissociates to yield enzyme (E) plus product (P).

          k1       k3
   E + S ----} ES ----} E + P
        {----    {----
	      k2       k4

<?xml version = '1.0' ?>

<model>

<title><basic> Enzyme Kinetics </basic></title>

<structure>
<name> Model </name>

<variables> ===================================

The principal parameters are reaction
rate constants.

<parm>
  <name> K1 </name>
  <val> 0.10 </val>
</parm>

<parm>
  <name> K2 </name>
  <val> 0.01 </val>
</parm>

<parm>
  <name> K3 </name>
  <val> 0.10 </val>
</parm>

<parm>
  <name> K4 </name>
  <val> 0.01 </val>
</parm>

</variables>

<equations> ===================================

There are 4 differential equations,
representing

   E  - Enzyme
   S  - Substrate
   ES - Enzyme/Substrate Complex
   P  - Product

<diffeq>
  <name> E </name>
  <integralname> E </integralname>
  <initialval> 1.0 </initialval>
  <dervname> EDot </dervname>
  <errorlim> 0.01 </errorlim>
</diffeq>

<diffeq>
  <name> S </name>
  <integralname> S </integralname>
  <initialval> 1.0 </initialval>
  <dervname> SDot </dervname>
  <errorlim> 0.01 </errorlim>
</diffeq>

<diffeq>
  <name> ES </name>
  <integralname> ES </integralname>
  <initialval> 0.0 </initialval>
  <dervname> ESDot </dervname>
  <errorlim> 0.01 </errorlim>
</diffeq>

<diffeq>
  <name> P </name>
  <integralname> P </integralname>
  <initialval> 0.0 </initialval>
  <dervname> PDot </dervname>
  <errorlim> 0.01 </errorlim>
</diffeq>

</equations>

<definitions> =================================

<block><name> Dervs </name> ===================

<def>
  <name> EDot </name>
  <val>
      ( K2 * ES )
    + ( K3 * ES )
    - ( K1 * E * S )
    - ( K4 * E * P )
  </val>
</def>

<def>
  <name> SDot </name>
  <val>
      ( K2 * ES )
    - ( K1 * E * S )
  </val>
</def>

<def>
  <name> ESDot </name>
  <val>
      ( K1 * E * S )
    + ( K4 * E * P )
    - ( K2 * ES )
    - ( K3 * ES )
  </val>
</def>

<def>
  <name> PDot </name>
  <val>
      ( K3 * ES )
    - ( K4 * E * P )
  </val>
</def>

</block>

</definitions>
</structure>

<math> ========================================

  <dervs> Model.Dervs </dervs>

</math>

<control> =====================================


<gofor>
  <solutionint> 10 </solutionint>
  <displayint> 2 </displayint>
</gofor>

<gofor>
  <solutionint> 50 </solutionint>
  <displayint> 2 </displayint>
</gofor>


</control>

<display> =====================================

<panel><name> Enzyme Kinetics </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<showvalue>
   <row> 0.5 </row><col> 40 </col>
   <name> System.X </name>
   <format> <integer/> </format>
   <label> Time </label>
</showvalue>

<showvalue>
   <row> 2 </row><col> 1 </col>
   <name> E </name>
   <format><decimal> 2 </decimal></format>
   <label> E </label>
</showvalue>

<showvalue>
   <row> 3 </row><col> 1 </col>
   <name> S </name>
   <format><decimal> 2 </decimal></format>
   <label> S </label>
</showvalue>

<showvalue>
   <row> 4 </row><col> 1 </col>
   <name> ES </name>
   <format><decimal> 2 </decimal></format>
   <label> ES </label>
</showvalue>

<showvalue>
   <row> 5 </row><col> 1 </col>
   <name> P </name>
   <format><decimal> 2 </decimal></format>
   <label> P </label>
</showvalue>

<repeatlist>
  <name> ForwardK </name>
  <repeat>
    <reps> 20 </reps>
    <stepsize> 0.01 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 6.6 </row><col> 1 </col>
  <name> K1 </name>
  <listname> ForwardK </listname>
  <label> K1 </label>
</slidebar>

<repeatlist>
  <name> BackwardK </name>
  <repeat>
    <reps> 20 </reps>
    <stepsize> 0.001 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 8.2 </row><col> 1 </col>
  <name> K2 </name>
  <listname> BackwardK </listname>
  <label> K2 </label>
</slidebar>

<slidebar>
  <row> 9.8 </row><col> 1 </col>
  <name> K3 </name>
  <listname> ForwardK </listname>
  <label> K3 </label>
</slidebar>

<slidebar>
  <row> 11.4 </row><col> 1 </col>
  <name> K4 </name>
  <listname> BackwardK </listname>
  <label> K4 </label>
</slidebar>

<showgraph>
  <row> 2.0 </row><col> 30 </col>
  <high> 13 </high><wide> 28 </wide>
  <leftmargin> 4 </leftmargin>
  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 50 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> E </name>
      <label> E </label>
      <linecolor> BLACK </linecolor>
    </yvar>
    <yvar>
      <name> S </name>
      <label> S </label>
      <linecolor> BLUE </linecolor>
    </yvar>
    <yvar>
      <name> ES </name>
      <label> ES </label>
      <linecolor> MAGENTA </linecolor>
    </yvar>
    <yvar>
      <name> P </name>
      <label> P </label>
      <linecolor> RED </linecolor>
    </yvar>
    <scale><min> 0 </min><max> 1 </max>
    </scale>
  </yaxis>
</showgraph>

<infobutton>
  <row> 16 </row><col> 30 </col>
  <label> Names </label>
  <line> E - Enzyme </line>
  <line> S - Substrate </line>
  <line> ES - Enzyme/Substrate Complex </line>
  <line> P - Product </line>
  <line>  </line>
  <line> K - Rate Constant </line>
  <line>  </line>
  <line> K1 - K for E + S ---} ES </line>
  <line> K2 - K for E + S {--- ES </line>
  <line> K3 - K for ES ---} E + P </line>
  <line> K4 - K for ES {--- E + P </line>
</infobutton>

</panel>
</display>

</model>

End
Epidemic

Created : 24-Aug-96
Last Modified : 28-Nov-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

Members of the population reside in one of
three classes:

# Healthy (H)
# Contagious (C)
# Recovered (R)

Contagious persons can infect healthy persons.
Recovered persons are either dead or immune
and cannot be reinfected.

The independent variable is time in days.

The healthy, contagious and recovered populations
are equal to the integral over time of their net
rate of change.

<?xml version = '1.0' ?>

<model>

<title><basic> Epidemic </basic></title>

<structure>
<name> Model </name>

<variables> ======================================

The rate of contagion is equal to a virulence factor
multiplied by the populations of healthy and
contagious persons.  Contagion decreases the healthy
population.

<parm><name> Virulence </name><val> 0.001 </val></parm>

The rate of recovery is equal to a recovery factor
multiplied by the contagious population.

<parm><name> RecoverySpeed </name><val> 0.1 </val></parm>

We can also define the initial infection count.

<parm><name> InitCount </name><val> 5.0 </val></parm>

</variables>

<equations> =====================================

<diffeq>
  <name> H </name>
  <integralname> H </integralname>
  <initialval> 1000.0 </initialval>
  <dervname> dH/dt </dervname>
  <errorlim> 1.0 </errorlim>
</diffeq>

<diffeq>
  <name> C </name>
  <integralname> C </integralname>
  <initialval> 5.0 </initialval>
  <dervname> dC/dt </dervname>
  <errorlim> 1.0 </errorlim>
</diffeq>

<diffeq>
  <name> R </name>
  <integralname> R </integralname>
  <initialval> 0.0 </initialval>
  <dervname> dR/dt </dervname>
  <errorlim> 1.0 </errorlim>
</diffeq>

</equations>

<definitions> ===================================

<block><name> Parms </name> ===================

<def>
  <name> C </name>
  <val> InitCount </val>
</def>

</block>

<block><name> Dervs </name> =====================

<def>
  <name> dH/dt </name>
  <val> - ( Virulence * H * C ) </val>
</def>

<def>
  <name> dR/dt </name>
  <val> RecoverySpeed * C </val>
</def>

<def>
  <name> dC/dt </name>
  <val> - dH/dt - dR/dt </val>
</def>

</block>

</definitions>
</structure>

<math>
<parms> Model.Parms </parms>
<dervs> Model.Dervs </dervs>
</math>

<control> ========================================

<gofor>
  <solutionint> 20 </solutionint>
  <displayint> 0.5 </displayint>
</gofor>

</control>

<display> =========================================

<panel><name> Epidemic Model </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<showbargraph>
  <row> 2.4 </row><col> 1.0 </col>
  <high> 5 </high><wide> 26 </wide>
  <leftmargin> 3 </leftmargin>
  <title> People </title>
  <showinitialvalues/>

  <bar><name> H </name><label> H </label><color> BLUE </color></bar>
  <bar><name> C </name><label> C </label><color> RED </color></bar>
  <bar><name> R </name><label> R </label><color> BLACK </color></bar>

  <scale><min> 0 </min><max> 1000 </max></scale>
</showbargraph>

<showvalue>
   <row> 8.0 </row><col> 1 </col>
   <name> H </name>
   <format><integer/></format>
   <label> Healthy </label>
</showvalue>

<showvalue>
   <row> 9.0 </row><col> 1 </col>
   <name> C </name>
   <format><integer/></format>
   <label> Contagious </label>
</showvalue>

<showvalue>
   <row> 10.0 </row><col> 1 </col>
   <name> R </name>
   <format><integer/></format>
   <label> Recovered </label>
</showvalue>

Initial Infection

<groupbox>
   <row> 11.5 </row><col> 1 </col>
   <high> 3.5 </high>
   <wide> 24 </wide>
   <title> Initial Infection </title>

<repeatlist>
  <name> InitCount </name>
  <repeat><reps> 25 </reps><stepsize> 1 </stepsize></repeat>
</repeatlist>

<slidebar>
  <row> 1.6 </row><col> 1 </col>
  <wide> 8 </wide>
  <name> InitCount </name>
  <listname> InitCount </listname>
  <label> Count </label>
</slidebar>

</groupbox>

Virulence

<groupbox>
   <row> 15.5 </row><col> 1 </col>
   <high> 3.5 </high>
   <wide> 24 </wide>
   <title> Virulence </title>

<maplist>
  <name> Virulence </name>
  <map><val> 0.0    </val><img> None </img></map>
  <map><val> 0.0004 </val><img> Low </img></map>
  <map><val> 0.001  </val><img> Normal </img></map>
  <map><val> 0.004  </val><img> High </img></map>
  <map><val> 0.01   </val><img> Very High </img></map>
</maplist>

<slidebar>
  <row> 1.6 </row><col> 1 </col>
  <name> Virulence </name>
  <listname> Virulence </listname>
  <nolabel/>
</slidebar>

</groupbox>

Recovery

<groupbox>
   <row> 19.5 </row><col> 1 </col>
   <high> 3.5 </high>
   <wide> 24 </wide>
   <title> Recovery </title>

<maplist>
  <name> RecoverySpeed </name>
  <map><val> 0.0  </val><img> None </img></map>
  <map><val> 0.04 </val><img> Slow </img></map>
  <map><val> 0.1  </val><img> Normal </img></map>
  <map><val> 0.4  </val><img> Fast </img></map>
  <map><val> 1.0  </val><img> Very Fast </img></map>
</maplist>

<slidebar>
  <row> 1.6 </row><col> 1 </col>
  <name> RecoverySpeed </name>
  <listname> RecoverySpeed </listname>
  <nolabel/>
</slidebar>

</groupbox>

===============================================

<showclock>
  <row> 0.5 </row><col> 28 </col>
  <name> System.X </name>
  <timebase> DAY </timebase>
  <days/>
</showclock>

<showgraph>
  <row> 2.0 </row><col> 28 </col>
  <high> 12 </high><wide> 32 </wide>
  <leftmargin> 4 </leftmargin>
  <xaxis>
     <name> System.X </name><label> Days </label>
     <scale><min> 0 </min><max> 20 </max></scale>
  </xaxis>
  <yaxis>
    <yvar><name> H </name><label> H </label><linecolor> BLUE </linecolor></yvar>
    <yvar><name> C </name><label> C </label><linecolor> RED </linecolor></yvar>
    <yvar><name> R </name><label> R </label></yvar>

    <scale><min> 0 </min><max> 1000 </max></scale>
  </yaxis>
</showgraph>

<infobutton>
  <row> 14.5 </row><col> 28 </col>
  <label> Populations </label>
  <line> H - Healthy </line>
  <line> C - Contagious </line>
  <line> R - Recovered and Immune </line>
</infobutton>

</panel>
</display>

</model>

EndFirst-Order DE

Created : 25-Aug-96
Last Modified : 01-Dec-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

<?xml version = '1.0' ?>

DE is a contraction of differential equation.

The dependent variable here is Y and the independent
variable is X.  The equation is

   dY/dX = - K * Y

with Y0 being the initial value of Y.

K is known as the rate constant. When the independent
variable is time, the time constant is the reciprocal
of the rate constant.

   Tau = 1 / K

When X = Tau, Y = 0.37 * Y0.

The half-time is the time required for Y to go
one-half way from its initial value to its final
value.

   t 1/2 = 0.69 * Tau

The exact solution of this equation is

   Exact Y = Y0 * exp (-K * X)

The error in the calculated solution is

   Error = Exact Y - Calculated Y

<model>

<title><basic> First-Order DE </basic></title>

<structure> <name> Model </name>

<variables> ------------------------------------

<parm><name> K </name><val> 0.1 </val></parm>
<var><name> Tau </name></var>
<var><name> t1/2 </name></var>
<var><name> ExactY </name></var>
<var><name> Error </name></var>

</variables>

<equations> ------------------------------------

<diffeq>
  <name> Y </name>
  <integralname> Y </integralname>
  <initialval> 1.0 </initialval>
  <dervname> dY/dX </dervname>
</diffeq>

</equations>

<definitions> ----------------------------------

<block><name> Parms </name>

<def>
  <name> Tau </name>
  <val> INVERT K </val>
</def>

<def>
  <name> t1/2 </name>
  <val> 0.69 * Tau </val>
</def>

</block>

<block><name> Dervs </name>

<def>
  <name> dY/dX </name>
  <val> - K * Y </val>
</def>

<def>
  <name> ExactY </name>
  <val> EXP ( - K * System.X ) </val>
</def>

<def>
  <name> Error </name>
  <val> ExactY - Y </val>
</def>

</block>
</definitions>
</structure>

<math> --------------------------------------

<parms> Model.Parms </parms>
<dervs> Model.Dervs </dervs>

</math>

<control> ---------------------------------------


<gofor>
  <solutionint> 10 </solutionint>
  <displayint> 2 </displayint>
  <menuitem> Big Steps </menuitem>
</gofor>

<gofor>
  <solutionint> 10 </solutionint>
  <displayint> 1 </displayint>
  <menuitem> Medium Steps </menuitem>
</gofor>

<gofor>
  <solutionint> 10 </solutionint>
  <displayint> 0.2 </displayint>
  <menuitem> Small Steps </menuitem>
</gofor>


</control>

<display> ---------------------------------------

<panel> <name> First-Order DE </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<infobutton>
  <row> 14 </row><col> 1 </col>
  <label> The Equation </label>
  <line> The equation is </line>
  <line>  </line>
  <line> dY / dX = - K * Y </line>
  <line>  </line>
  <line> X is the independent variable, </line>
  <line> Y is the dependent variable, </line>
  <line> and Y0 is the initial value </line>
  <line> of Y. </line>
</infobutton>

<infobutton>
  <row> 15.6 </row><col> 1 </col>
  <label> Rate Constant </label>
  <line> K is known as the rate </line>
  <line> constant in the equation </line>
  <line>  </line>
  <line> dY / dX = - K * Y </line>
  <line>  </line>
  <line> When the independent variable </line>
  <line> is time, the time constant </line>
  <line> (Tau) is the reciprocal of </line>
  <line> the rate constant. </line>
  <line>  </line>
  <line> Tau = 1 / K </line>
  <line>  </line>
  <line> When X = Tau, Y = 0.37 * Y0. </line>
  <line>  </line>
  <line> The half-time is the time </line>
  <line> required for Y to go one-half </line>
  <line> way from its initial value to </line>
  <line> its final value. </line>
  <line>  </line>
  <line> t 1/2 = 0.69 * Tau </line>
</infobutton>

<infobutton>
  <row> 17.2 </row><col> 1 </col>
  <label> Exact Solution </label>
  <line> The equation </line>
  <line>  </line>
  <line> dY / dX = - K * Y with </line>
  <line> initial Y0 </line>
  <line>  </line>
  <line> is one of only a few </line>
  <line> differential equations that </line>
  <line> have known exact solutions. </line>
  <line> The exact solution is </line>
  <line>  </line>
  <line> Y = Y0 * exp ( -K * X ) </line>
</infobutton>

<infobutton>
  <row> 18.8 </row><col> 1 </col>
  <label> Solution Error </label>
  <line> The calculated solution is </line>
  <line> only approximate.  The error </line>
  <line> in this calculation is the </line>
  <line> difference between the exact </line>
  <line> solution and the calculated </line>
  <line> solution. </line>
</infobutton>

<showvalue>
  <row> 2 </row><col> 1 </col>
  <name> Y </name>
  <format><decimal> 1 </decimal></format>
  <label> Y </label>
</showvalue>

<showvalue>
  <row> 3 </row><col> 1 </col>
  <name> dY/dX </name>
  <format><decimal> 2 </decimal></format>
  <label> dY/dX </label>
</showvalue>

<showvalue>
  <row> 4 </row><col> 1 </col>
  <name> ExactY </name>
  <format><decimal> 1 </decimal></format>
  <label> Exact Y </label>
</showvalue>

<showvalue>
  <row> 5 </row><col> 1 </col>
  <name> Error </name>
  <format><decimal> 3 </decimal></format>
  <label> Error </label>
</showvalue>

Rate constant

<groupbox>
   <row> 7 </row><col> 1 </col>
   <high> 5.5 </high><wide> 26 </wide>
   <title> Rate Constant </title>

<repeatlist>
  <name> RateConst </name>
  <firstval> 0.01 </firstval>

  <repeat>
    <reps> 19 </reps>
    <stepsize> 0.01 </stepsize>
  </repeat>

  <repeat>
    <reps> 8 </reps>
    <stepsize> 0.1 </stepsize>
  </repeat>

</repeatlist>

<slidebar>
  <row> 1.6 </row><col> 1 </col>
  <name> K </name>
  <listname> RateConst </listname>
  <label> K </label>
</slidebar>

<showvalue>
  <row> 3.0 </row><col> 1 </col>
  <name> Tau </name>
  <format><decimal> 1 </decimal></format>
  <label> Time Constant </label>
</showvalue>

<showvalue>
  <row> 4.0 </row><col> 1 </col>
  <name> t1/2 </name>
  <format><decimal> 1 </decimal></format>
  <label> Half-Life </label>
</showvalue>

</groupbox>

Right-hand Side ---------------------------------

<showvalue>
  <row> 0.5 </row><col> 37 </col>
  <name> System.X </name>
  <format><decimal> 1 </decimal></format>
</showvalue>

<symbol>
  <name> Box </name>
  <style> BOX </style>
  <color> RED </color>
  <size> 2 </size>
  <linewidth> 1 </linewidth>
</symbol>

<showgraph>
  <row> 2.5 </row><col> 28 </col>
  <high> 7 </high><wide> 32 </wide>
  <leftmargin> 8 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> X </label>
    <scale><min> 0 </min><max> 20 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> Y </name>
      <label> Y </label>
    </yvar>
			   
    <yvar>
      <name> ExactY </name>
      <label> Exact Y </label>
      <linetype> NULL </linetype>
      <linecolor> RED </linecolor>
      <symbolname> Box </symbolname>
    </yvar>

    <scale><min> 0 </min><max> 1 </max></scale>
  </yaxis>

</showgraph>

<showgraph>
  <row> 10.0 </row><col> 28 </col>
  <high> 7 </high><wide> 32 </wide>
  <leftmargin> 8 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> X </label>
    <scale><min> 0 </min><max> 20 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> dY/dX </name>
      <label> dY/dX </label>
      <linecolor> BLUE </linecolor>
    </yvar>

    <scale><min> -0.1 </min><max> 0 </max></scale>
  </yaxis>

</showgraph>

<showgraph>
  <row> 17.5 </row><col> 28 </col>
  <high> 7 </high><wide> 32 </wide>
  <leftmargin> 8 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> X </label>
    <scale><min> 0 </min><max> 20 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> Error </name>
      <label> Error </label>
      <linetype> DOT </linetype>
      <linecolor> MAGENTA </linecolor>
    </yvar>
			   
    <scale><min> -0.01 </min><max> +0.01 </max></scale>
  </yaxis>

</showgraph>

</panel>
</display>

</model>

EndFurnace

Created : 24-Aug-96
Last Modified : 01-Dec-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

The house cools down, the thermostat clicks the
furance on.  The furnace heats the house, the
thermostat click offs.

The temperature inside a house is directly
proportional to the amount of heat in the house.
The amount of heat is the integral over time
of the net accumulation of heat in the house.
Net accumulation is the difference between heat
added by the furnace and heat lost to the
outside world.  The furnace is controlled by
a thermostat.

The independent variable is time in minutes.

Units for heat are kilocalories.

<?xml version = '1.0' ?>

<model>

<title><basic> Furnace </basic></title>

<structure>
<name> Model </name>

<variables> =====================================

Units for house volume are cubic meters.

<parm>
  <name> Volume </name>
  <val> 400 </val>
</parm>


A thermostat senses temperature and
controls the furnace.

<parm>
  <name> TempOn </name>
  <val> 75 </val>
</parm>

<parm>
  <name> TempOff </name>
  <val> 80 </val>
</parm>

<parm>
  <name> Burner </name>
  <val> 25 </val>
</parm>

<parm>
  <name> Leak </name>
  <val> 0.2 </val>
</parm>

<parm>
  <name> EnvTemp </name>
  <val> 40 </val>
</parm>

<var>
  <name> Switch </name>
  <val> 0 </val>
</var>

<var>
  <name> FirstTime </name>
  <val> TRUE </val>
</var>

<var>
  <name> MinTemp </name>
  <val> UNDEFINED </val>
</var>

<var>
  <name> MaxTemp </name>
  <val> UNDEFINED </val>
</var>

<var>
  <name> TempK </name>
</var>

<var>
  <name> TempC </name>
</var>

<var>
  <name> TempF </name>
</var>

<var>
  <name> HeatGain </name>
  <val> 0.0 </val>
</var>

<var>
  <name> HeatLoss </name>
</var>

</variables>

<equations> ====================================

<diffeq>
  <name> Heat </name>
  <integralname> Heat </integralname>
  <initialval> 26500.0 </initialval>
  <dervname> NetHeat </dervname>
  <errorlim> 100.0 </errorlim>
</diffeq>

<diffeq>
  <name> FuelUsed </name>
  <integralname> FuelUsed </integralname>
  <initialval> 0.0 </initialval>
  <dervname> dFuelUsed/dt </dervname>
</diffeq>

</equations>

<definitions> ==================================

<block><name> Dervs </name> ====================

It takes 4.98 kCal to heat 22.4 kMol of air 1
degree K.  22.4 kMol of air occupies 22.4 cubic
meters.  Thus, 1 kiloCal of heat in a cubic
meter has a temperature of 4.5 degrees K.

<def>
  <name> TempK </name>
  <val> 4.5 * Heat / Volume </val>
</def>

<def>
  <name> TempC </name>
  <val> TempK - 273 </val>
</def>

<def>
  <name> TempF </name>
  <val> ( ( 9 / 5 ) * TempC ) + 32 </val>
</def>

<if>
  <test> TempF GE TempOff </test>
  <true>
    <def>
      <name> Switch </name>
      <val> FALSE </val>
    </def>
    <def>
      <name> HeatGain </name>
      <val> 0 </val>
    </def>
  </true>
</if>

<if>
  <test> TempF LE TempOn </test>
  <true>
    <def>
      <name> Switch </name>
      <val> TRUE </val>
    </def>
    <def>
      <name> HeatGain </name>
      <val> Burner </val>
    </def>
  </true>
</if>

<def>
  <name> dFuelUsed/dt </name>
  <val> HeatGain </val>
</def>

<def>
  <name> HeatLoss </name>
  <val> Leak * ( TempF - EnvTemp ) </val>
</def>

<def>
  <name> NetHeat </name>
  <val> HeatGain - HeatLoss </val>
</def>

<if>
  <test> FirstTime </test>
  <true>
    <def>
      <name> FirstTime </name>
      <val> FALSE </val>
    </def>
    <def>
      <name> MinTemp </name>
      <val> TempF </val>
    </def>
    <def>
      <name> MaxTemp </name>
      <val> TempF </val>
    </def>
  </true>
  <false>
    <conditional>
      <name> MaxTemp </name>
      <test> TempF GT MaxTemp </test>
      <true> TempF </true>
    </conditional>
    <conditional>
      <name> MinTemp </name>
      <test> TempF LT MinTemp </test>
      <true> TempF </true>
    </conditional>
  </false>
</if>

</block>

</definitions>
</structure>

<math> ========================================

  <dervs> Model.Dervs </dervs>

</math>

<control> =====================================


<gofor>
  <solutionint> 60 </solutionint>
  <displayint> 2 </displayint>
</gofor>

<gofor>
  <solutionint> 120 </solutionint>
  <displayint> 2 </displayint>
</gofor>


</control>

<display> =========================================
<panel>

<name> Furnace Heats a House </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<showclock>
   <row> 0.5 </row><col> 24 </col>
   <name> System.X </name>
</showclock>

<maplist>
  <name> Switch </name>
  <map><val> 0 </val><img> Off </img></map>
  <map><val> 1 </val><img> On </img></map>
</maplist>

<infobutton>
  <row> 0.5 </row><col> 41 </col>
  <label> Math </label>
  <line> Heat - kiloCalories </line>
  <line> Time - Min </line>
  <line>  </line>
  <line> 4.98 kCal are required to </line>
  <line> heat 22.4 kMol or 22.4 M^3 </line>
  <line> of air 1 degree K. Thus, 1 </line>
  <line> kCal of heat in 1 M^3 of </line>
  <line> air has a temperature of </line>
  <line> 4.5 degrees Kelvin. </line>
</infobutton>

<showvalue>
   <row> 2 </row><col> 1 </col>
   <name> TempF </name>
   <format> <integer/> </format>
   <label> Temperature ( F ) </label>
</showvalue>

<showvalue>
   <row> 2 </row><col> 21 </col>
   <name> TempC </name>
   <format> <integer/> </format>
   <label> ( C ) </label>
</showvalue>

<showvalue>
   <row> 2 </row><col> 30 </col>
   <name> TempK </name>
   <format> <integer/> </format>
   <label> ( K ) </label>
</showvalue>

<infobutton>
  <row> 2 </row><col> 41 </col>
  <label> Conversions </label>
  <line> C - Centigrade </line>
  <line> F - Fahrenheit </line>
  <line> K - Kelvin </line>
  <line>  </line>
  <line> C = K - 273 </line>
  <line> F = ( 9 / 5 ) * C + 32 </line>
</infobutton>

<repeatlist>
  <name> Temps </name>
  <firstval> -100 </firstval>
  <repeat>
    <reps>16  </reps>
    <stepsize> 10 </stepsize>
  </repeat>
  <repeat>
    <reps> 30 </reps>
    <stepsize> 1 </stepsize>
  </repeat>
  <repeat>
    <reps> 5 </reps>
    <stepsize> 10 </stepsize>
  </repeat>
</repeatlist>

Thermostat

<groupbox>
   <row> 3.5 </row><col> 1 </col>
   <high> 5.5 </high><wide> 30 </wide>
   <title> Thermostat </title>

<slidebar>
  <row> 1.4 </row><col> 1 </col>
  <name> TempOff </name>
  <listname> Temps </listname>
  <label> Turn Off </label>
</slidebar>

<slidebar>
  <row> 2.8 </row><col> 1 </col>
  <name> TempOn </name>
  <listname> Temps </listname>
  <label> Turn On </label>
</slidebar>

<showvalue>
   <row> 4.2 </row><col> 1 </col>
   <name> Switch </name>
   <format><list> Switch </list></format>
   <label> Switch </label>
</showvalue>

</groupbox>

Furnace

<groupbox>
   <row> 9.5 </row><col> 1 </col>
   <high> 4.5 </high><wide> 30 </wide>
   <title> Furnace </title>

<repeatlist>
  <name> Burner </name>
  <repeat>
    <reps> 20 </reps>
    <stepsize> 5 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 1.5 </row><col> 1 </col>
  <name> Burner </name>
  <listname> Burner </listname>
  <label> Burner </label>
</slidebar>

<showvalue>
   <row> 3.0 </row><col> 1 </col>
   <name> FuelUsed </name>
   <format><decimal> 0 </decimal></format>
   <label> Fuel Used </label>
</showvalue>

</groupbox>

Environment

<groupbox>
   <row> 14.5 </row><col> 1 </col>
   <high> 4.8 </high><wide> 30 </wide>
   <title> Environment </title>

<slidebar>
  <row> 1.6 </row><col> 1 </col>
  <name> EnvTemp </name>
  <listname> Temps </listname>
  <label> Outdoor Temp </label>
</slidebar>

<maplist>
  <name> Insulate </name>
  <map><val> 0.04 </val><img> Best </img></map>
  <map><val> 0.10 </val><img> Good </img></map>
  <map><val> 0.20 </val><img> Normal </img></map>
  <map><val> 0.50 </val><img> Poor </img></map>
  <map><val> 1.00 </val><img> None </img></map>
</maplist>

<slidebar>
  <row> 3.2 </row><col> 1 </col>
  <name> Leak </name>
  <listname> Insulate </listname>
  <label> Insulation </label>
</slidebar>

</groupbox>

Reference

<infobutton>
  <row> 20.4 </row><col> 1 </col>
  <label> Reference </label>
  <line> Shortley, G. and D. Williams </line>
  <line> 1967 </line>
  <line> Principles of College Physics </line>
  <line> Prentice - Hall </line>
  <line> Englewood Cliffs, NJ </line>
</infobutton>

Right-hand side ===============================

<showgraph>
  <row> 4.0 </row><col> 32 </col>
  <high> 8 </high><wide> 30 </wide>
  <leftmargin> 6 </leftmargin>
  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 120 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> TempF </name>
      <label> Temp </label>
      <linecolor> RED </linecolor>
    </yvar>
    <scale><min> 70 </min><max> 80 </max><inc> 10 </inc></scale>
  </yaxis>
</showgraph>

<showgraph>
  <row> 12.5 </row><col> 32 </col>
  <high> 4.7 </high><wide> 30 </wide>
  <leftmargin> 6 </leftmargin>
  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 120 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> Switch </name>
      <label> Switch </label>
      <linecolor> BLUE </linecolor>
    </yvar>
    <scale><min> 0 </min><max> 1 </max></scale>
  </yaxis>
</showgraph>

<showvalue>
   <row> 18.0 </row><col> 32 </col>
   <name> MinTemp </name>
   <format> <integer/> </format>
   <label> Temperature Min </label>
</showvalue>

<showvalue>
   <row> 18.0 </row><col> 51 </col>
   <name> MaxTemp </name>
   <format> <integer/> </format>
   <label> Max </label>
</showvalue>

</panel>
</display>

</model>

End
Glucose

Created : 25-Aug-96
Last Modified : 01-Dec-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

High glucose concentration in the extratracellular
fluid stimulates insulin release by the pancreas.
Insulin, in turn, stimulates cellular uptake of
glucose.  Low glucose concentration inhibits
insulin secretion and stimulates hepatic release
of glucose.

This model is scaled to a 70 kG adult.

The independent variable is time in minutes.

The quantities of glucose and insulin in the
extracellular fluid volume are the integrals over
time of their net rate of change.  Glucose
concentration is reported in G/dL.

<?xml version = '1.0' ?>

<model>

<title><basic> Glucose </basic></title>

<structure>
<name> Model </name>

<variables> ===================================

We have a glucose pump and an insulin pump.  Each
has a switch and setting.  When the switch is on,
rate is equal to the setting.

<parm>
  <name> GSwitch </name>
  <val> FALSE </val>
</parm>

<parm>
  <name> GSetting </name>
  <val> 0 </val>
</parm>

<parm>
  <name> ISwitch </name>
  <val> FALSE </val>
</parm>

<parm>
  <name> ISetting </name>
  <val> 0 </val>
</parm>

The volume of distribution of glucose and insulin
is the extracellular fluid.

<parm>
  <name> ECFV </name>
  <val> 1.5e4 </val>
</parm>

Next is an insulin clearance constant.

<parm>
  <name> InsClear </name>
  <val> 770 </val>
</parm>

<var>
  <name> GRate </name>
</var>

<var>
  <name> IRate </name>
</var>

<var>
  <name> [G] </name>
</var>

<var>
  <name> [I] </name>
</var>

<var>
  <name> GLiver </name>
</var>

<var>
  <name> GCells </name>
</var>

<var>
  <name> ISecrete </name>
</var>

<var>
  <name> IClear </name>
</var>

</variables>

<equations> ==================================

<diffeq>
  <name> G </name>
  <integralname> G </integralname>
  <initialval> 1.5e4 </initialval>
  <dervname> dG/dt </dervname>
  <errorlim> 1e2 </errorlim>
</diffeq>

<diffeq>
  <name> I </name>
  <integralname> I </integralname>
  <initialval> 1.5e5 </initialval>
  <dervname> dI/dt </dervname>
  <errorlim> 1e3 </errorlim>
</diffeq>

</equations>

<functions> ====================================

<curve>
  <name> GLiverCurve </name>
  <point>
    <x>   0 </x><y> 350 </y><slope> 0 </slope>
  </point>
  <point>
    <x>  10 </x><y> 150 </y><slope> -2.5 </slope>
  </point>
  <point>
    <x> 200 </x><y>   0 </y><slope> 0 </slope>
  </point>
</curve>

<curve>
  <name> GCellsCurve </name>
  <point>
    <x>   0 </x><y>  1.1 </y><slope> 0 </slope>
  </point>
  <point>
    <x>  10 </x><y>  1.5 </y><slope> 0.06 </slope>
  </point>
  <point>
    <x> 200 </x><y> 11.0 </y><slope> 0 </slope>
  </point>
</curve>

<curve>
  <name> ISecreteCurve </name>
  <point>
    <x>   0 </x><y>      0 </y><slope> 0 </slope>
  </point>
  <point>
    <x> 100 </x><y>   7700 </y><slope> 140 </slope>
  </point>
  <point>
    <x> 300 </x><y> 112000 </y><slope> 0 </slope>
  </point>
</curve>

</functions>

<definitions> =================================

<block><name> Parms </name> ===================

Control the pumps.

<conditional>
  <name> GRate </name>
  <test> GSwitch </test>
  <true> GSetting </true>
  <false> 0.0 </false>
</conditional>

<conditional>
  <name> IRate </name>
  <test> ISwitch </test>
  <true> ISetting </true>
  <false> 0.0 </false>
</conditional>

</block>

<block><name> Dervs </name> ===================

First, the concentrations

<def>
  <name> [G] </name>
  <val> ( G / ECFV ) * 100 </val>
</def>

<def>
  <name> [I] </name>
  <val> I / ECFV </val>
</def>

Sources of glucose are the liver and infusion.

<def>
  <name> GLiver </name>
  <val> GLiverCurve [ [I] ] </val>
</def>

Glucose is removed from the extracellular
fluid by cellular uptake.  Uptake is a
function of glucose and insulin concentration.

<def>
  <name> GCells </name>
  <val> ( GCellsCurve [ [I] ] ) * [G] </val>
</def>

The net rate of change of glucose is a function
of release by the liver, infusion, and cellular
uptake.

<def>
  <name> dG/dt </name>
  <val> GLiver + GRate - GCells </val>
</def>

The sources of insulin are secretion by the pancreas
and infusion.

<def>
  <name> ISecrete </name>
  <val> ISecreteCurve [ [G] ] </val>
</def>

Insulin elimination is a function of insulin
concentration.

<def>
  <name> IClear </name>
  <val> InsClear * [I] </val>
</def>

The rate of change of insulin is equal to pancreatic
secretion plus infusion minus insulin elimination.

<def>
  <name> dI/dt </name>
  <val> ISecrete + IRate - IClear </val>
</def>

</block>

</definitions>
</structure>

<math> ========================================

  <parms> Model.Parms </parms>
  <dervs> Model.Dervs </dervs>

</math>

<control> =====================================


<gofor>
  <solutionint> 2 </solutionint>
  <displayint> 2 </displayint>
  <menuitem> 2 Min </menuitem>
</gofor>

<gofor>
  <solutionint> 10 </solutionint>
  <displayint> 2 </displayint>
  <menuitem> 10 Min </menuitem>
</gofor>

<gofor>
  <solutionint> 30 </solutionint>
  <displayint> 2 </displayint>
  <menuitem> 30 Min </menuitem>
</gofor>

<gofor>
  <solutionint> 120 </solutionint>
  <displayint> 2 </displayint>
  <menuitem> 120 Min </menuitem>
</gofor>


</control>

<display> =========================================
<panel>

<name> Glucose - Insulin Interaction </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<maplist>
  <name> Switch </name>
  <map><val> 0 </val><img> Off </img></map>
  <map><val> 1 </val><img> On </img></map>
</maplist>

<showvalue>
   <row> 2.5 </row><col> 1 </col>
   <name> [G] </name>
   <format> <integer/> </format>
   <label> [ Glucose ] </label>
</showvalue>

<showvalue>
   <row> 3.5 </row><col> 3 </col>
   <name> GLiver </name>
   <format> <integer/> </format>
   <label> Hepatic Release </label>
</showvalue>

<showvalue>
   <row> 4.5 </row><col> 3 </col>
   <name> GRate </name>
   <format> <integer/> </format>
   <label> Infusion </label>
</showvalue>

<showvalue>
   <row> 5.5 </row><col> 3 </col>
   <name> GCells </name>
   <format> <integer/> </format>
   <label> Cell Uptake </label>
</showvalue>

<showvalue>
   <row> 7.5 </row><col> 1 </col>
   <name> [I] </name>
   <format> <integer/> </format>
   <label> [ Insulin ] </label>
</showvalue>

<showvalue>
   <row> 8.5 </row><col> 3 </col>
   <name> ISecrete </name>
   <format> <integer/> </format>
   <label> Panreatic Secretion </label>
</showvalue>

<showvalue>
   <row> 9.5 </row><col> 3 </col>
   <name> IRate </name>
   <format> <integer/> </format>
   <label> Infusion </label>
</showvalue>

<showvalue>
   <row> 10.5 </row><col> 3 </col>
   <name> IClear </name>
   <format> <integer/> </format>
   <label> Clearance </label>
</showvalue>

Glucose Pump

<groupbox>
   <row> 13.0 </row><col> 1 </col>
   <high> 4.8 </high><wide> 26 </wide>
   <title> Glucose Pump </title>

<radiobuttons>
  <row> 1.6 </row><col> 1 </col>
  <name> GSwitch </name>
  <listname> Switch </listname>
  <label> Switch </label>
</radiobuttons>

<repeatlist>
  <name> GRate </name>
  <repeat>
    <reps> 20 </reps>
    <stepsize> 10 </stepsize>
  </repeat>
  <repeat>
    <reps> 8 </reps>
    <stepsize> 100 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 3.0 </row><col> 1 </col>
  <name> GSetting </name>
  <listname> GRate </listname>
  <label> Rate </label>
</slidebar>

</groupbox>

Right-hand Side ==============================

<showvalue>
   <row> 0.5 </row><col> 28 </col>
   <name> System.X </name>
   <label> Time ( Min ) </label>
</showvalue>

<showgraph>
  <row> 2.5 </row><col> 28 </col>
  <high> 10 </high><wide> 32 </wide>
  <leftmargin> 6 </leftmargin>
  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 120 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> [G] </name>
      <label> [ G ] </label>
      <linecolor> RED </linecolor>
    </yvar>
    <yvar>
      <name> [I] </name>
      <label> [ I ] </label>
      <linecolor> BLUE </linecolor>
    </yvar>
    <scale><min> 0 </min><max> 100 </max></scale>
  </yaxis>
</showgraph>

Insulin Pump

<groupbox>
   <row> 13.0 </row><col> 28 </col>
   <high> 4.8 </high><wide> 26 </wide>
   <title> Insulin Pump </title>

<radiobuttons>
  <row> 1.6 </row><col> 1 </col>
  <name> ISwitch </name>
  <listname> Switch </listname>
  <label> Switch </label>
</radiobuttons>

<repeatlist>
  <name> IRate </name>
  <repeat>
    <reps> 10 </reps>
    <stepsize> 1000 </stepsize>
  </repeat>
  <repeat>
    <reps> 9 </reps>
    <stepsize> 10000 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 3.0 </row><col> 1 </col>
  <name> ISetting </name>
  <listname> IRate </listname>
  <label> Rate </label>
</slidebar>

</groupbox>

</panel>
</display>

</model>

End
Harris-Benedict Metabolism

Created : 08-Feb-10
Last Modified : 08-Feb-10
Author : Tom Coleman
Copyright : 2010-2010
By : University of Mississippi Medical Center
Schema : DES V1.0

Harris and Benedict penned the equations that predict basal
energy exposure as a function of height, weight, age and
gender.

The most widely cited equations relating resting metabolism to
body size, age and gender are provided by Harris and Benedict.
The original source of these equations is rather obscure, so I
just cite MacDonald and Hildebrandt (MacDonald and Hildebrandt,
2003) who use the equations. These authors cite Harris and
Benedict (Harris and Benedict 1919).

There are two equations, one for males and one for females:

  Male: BEE = 66.473 + 13.7516 * W + 5.0033 * H � 6.755 * A
  Female: BEE = 655.0955 + 9.5634 * W + 1.8496 * H � 4.6756 * A

where

	BEE = Basal Energy Expenditure (kCal/Day)
	W = Weight (kG)
	H = Height (cM)
	A = Age (Years)

MacDonald, A. and L. Hildebrandt. Comparison of formulaic equations
to determine energy expenditure in the critically ill patient. Nutrition
19:233-239, 2003.

<?xml version = '1.0' ?>

<model>

<title><basic> Harris-Benedict Metabolism </basic></title>

<structure>
<name> Model </name>

<variables> ======================================

<parm><name> Male-K1 </name><val> 66.473 </val></parm>
<parm><name> Male-K2 </name><val> 13.7516 </val></parm>
<parm><name> Male-K3 </name><val> 5.0033 </val></parm>
<parm><name> Male-K4 </name><val> -6.755 </val></parm>

<parm><name> Male-W </name><val> 72.4 </val></parm>
<parm><name> Male-H </name><val> 178.0 </val></parm>
<parm><name> Male-A </name><val> 37.0 </val></parm>

<var><name> Male-BEE </name></var>

<parm><name> Male-Benchmark </name><val> 1702.7 </val></parm>
<var><name> Male-Ratio </name></var>

<parm><name> Female-K1 </name><val> 655.0955 </val></parm>
<parm><name> Female-K2 </name><val> 9.5634 </val></parm>
<parm><name> Female-K3 </name><val> 1.8496 </val></parm>
<parm><name> Female-K4 </name><val> -4.6756 </val></parm>

<parm><name> Female-W </name><val> 63.5 </val></parm>
<parm><name> Female-H </name><val> 165.0 </val></parm>
<parm><name> Female-A </name><val> 37.0 </val></parm>

<var><name> Female-BEE </name></var>

<parm><name> Female-Benchmark </name><val> 1394.6 </val></parm>
<var><name> Female-Ratio </name></var>

</variables>

<definitions> ===================================

<block><name> Parms </name> ===================

<def>
  <name> Male-BEE </name>
  <val>
           Male-K1
     + ( Male-K2 * Male-W )
     + ( Male-K3 * Male-H )
     + ( Male-K4 * Male-A )
  </val>
</def>

<def>
  <name> Male-Ratio </name>
  <val> Male-BEE / Male-Benchmark </val>
</def>

<def>
  <name> Female-BEE </name>
  <val>
           Female-K1
     + ( Female-K2 * Female-W )
     + ( Female-K3 * Female-H )
     + ( Female-K4 * Female-A )
  </val>
</def>

<def>
  <name> Female-Ratio </name>
  <val> Female-BEE / Female-Benchmark </val>
</def>

</block>

</definitions>
</structure>

<math>
<parms> Model.Parms </parms>
</math>

<display> =========================================

<panel><name> Harris-Benedict Metabolism </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<editbox>
  <row> 2.0 </row><col> 1.0 </col>
  <name> Male-W </name>
</editbox>

<editbox>
  <row> 3.6 </row><col> 1.0 </col>
  <name> Male-H </name>
</editbox>

<editbox>
  <row> 5.2 </row><col> 1.0 </col>
  <name> Male-A </name>
</editbox>

<showvalue>
  <row> 6.8 </row><col> 1.0 </col>
  <name> Male-BEE </name>
  <format><decimal> 1 </decimal></format>
</showvalue>

<showvalue>
  <row> 7.8 </row><col> 1.0 </col>
  <name> Male-Ratio </name>
  <format><decimal> 2 </decimal></format>
</showvalue>

<editbox>
  <row> 12.0 </row><col> 1.0 </col>
  <name> Female-W </name>
</editbox>

<editbox>
  <row> 13.6 </row><col> 1.0 </col>
  <name> Female-H </name>
</editbox>

<editbox>
  <row> 15.2 </row><col> 1.0 </col>
  <name> Female-A </name>
</editbox>

<showvalue>
  <row> 16.8 </row><col> 1.0 </col>
  <name> Female-BEE </name>
  <format><decimal> 1 </decimal></format>
</showvalue>

<showvalue>
  <row> 17.8 </row><col> 1.0 </col>
  <name> Female-Ratio </name>
  <format><decimal> 2 </decimal></format>
</showvalue>

</panel>
</display>

</model>

End                                                                       Heart P-V

Created : 25-Nov-96
Last Modified : 01-Dec-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

P-V data is in Guyton, p. 150, taken
from Starling.

There are no differential or implicit
equations in this model.  Instead,
independent variable X is the volume.
It is run through some algebraic equations
to get length and tension.

<?xml version = '1.0' ?>

<model>

<title><basic> Heart P-V Curve </basic></title>

<structure>
<name> Model </name>

<variables> ===================================

<parm>
  <name> Stiff </name>
  <val> 4.6 </val>
</parm>

<parm>
  <name> Inotrop </name>
  <val> 1.0 </val>
</parm>

<parm>
  <name> Thick </name>
  <val> 1.0 </val>
</parm>

<var>
  <name> R </name>
</var>

<var>
  <name> L </name>
</var>

<var>
  <name> RT </name>
</var>

<var>
  <name> BasAT </name>
</var>

<var>
  <name> AT </name>
</var>

<var>
  <name> TT </name>
</var>

<var>
  <name> PDynes </name>
</var>

<var>
  <name> P </name>
</var>

</variables>

<functions> ====================================

<curve>
  <name> BasAT </name>
  <point>
    <x> 1.0 </x><y> 0 </y><slope> 0 </slope>
  </point>
  <point>
    <x> 2.2 </x><y> 12e5 </y><slope> 0 </slope>
  </point>
  <point>
    <x> 3.7 </x><y> 0 </y><slope> 0 </slope>
  </point>
</curve>

</functions>

<definitions> =================================

<block><name> Dervs </name> ===================

<def>
  <name> R </name>
  <val> SQRT ( System.X / 21 ) </val>
</def>

<def>
  <name> L </name>
  <val> 0.88 * R </val>
</def>

<def>
  <name> RT </name>
  <val> EXP ( Stiff * L ) - 1 </val>
</def>

<def>
  <name> BasAT </name>
  <val> BasAT [ L ] </val>
</def>

<def>
  <name> AT </name>
  <val> Inotrop * BasAT </val>
</def>

<def>
  <name> TT </name>
  <val> AT + RT </val>
</def>

<conditional>
  <name> PDynes </name>
  <test> R GT 0 </test>
  <true> TT * Thick / R </true>
  <false> UNDEFINED </false>
</conditional>

<conditional>
  <name> P </name>
  <test> PDynes EQ UNDEFINED </test>
  <true> UNDEFINED </true>
  <false> PDynes / 1330 </false>
</conditional>

</block>

</definitions>
</structure>

<math>
  <dervs> Model.Dervs </dervs>
</math>

<control> ========================================

<gofor>
  <solutionint> 270 </solutionint>
  <displayint> 10 </displayint>
</gofor>

</control>

<display> =========================================
<panel>

<name> Heart Pressure-Volume </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<showvalue>
   <row> 2 </row><col> 1 </col>
   <name> System.X </name>
   <format> <integer/> </format>
   <label> Volume </label>
</showvalue>

<showvalue>
   <row> 3 </row><col> 1 </col>
   <name> R </name>
   <format><decimal> 1 </decimal></format>
   <label> Radius </label>
</showvalue>

<showvalue>
   <row> 4 </row><col> 1 </col>
   <name> L </name>
   <format><decimal> 1 </decimal></format>
   <label> Sarcomere Length </label>
</showvalue>

<showvalue>
   <row> 5 </row><col> 1 </col>
   <name> RT </name>
   <format> <integer/> </format>
   <label> RT </label>
</showvalue>

<showvalue>
   <row> 6 </row><col> 1 </col>
   <name> AT </name>
   <format> <integer/> </format>
   <label> AT </label>
</showvalue>

<showvalue>
   <row> 7 </row><col> 1 </col>
   <name> TT </name>
   <format> <integer/> </format>
   <label> TT </label>
</showvalue>

<showvalue>
   <row> 8 </row><col> 1 </col>
   <name> PDynes </name>
   <format> <integer/> </format>
   <label> PDynes </label>
</showvalue>

<showvalue>
   <row> 9 </row><col> 1 </col>
   <name> P </name>
   <format> <integer/> </format>
   <label> P </label>
</showvalue>

Right-hand Side ===============================

<showgraph>
  <row> 1.0 </row><col> 21 </col>
  <high> 16 </high><wide> 40 </wide>
  <leftmargin> 7 </leftmargin>
  <xaxis>
    <name> System.X </name>
    <label> Volume </label>
    <scale><min> 0 </min><max> 300 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> RT </name>
      <label> RT </label>
      <linecolor> BLUE </linecolor>
    </yvar>
    <yvar>
      <name> AT </name>
      <label> AT </label>
      <linecolor> RED </linecolor>
    </yvar>
    <yvar>
      <name> TT </name>
      <label> TT </label>
      <linecolor> BLACK </linecolor>
    </yvar>
    <scale><min> 0 </min><max> 3000000 </max><inc> 1000000 </inc></scale>
  </yaxis>
</showgraph>

</panel>
</display>

</model>

End
Low-Pass Filter

Created : 23-Aug-96
Last Modified : 01-Dec-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

A low-pass filter will smooth an oscillating
signal by attenuating higher frequency
oscillations while passing lower frequency
oscillations.

This model is a first-order, low-pass filter
which is implemented by the first-order
differential equation

   dOutput / dt = K * (Input - Output)

The independent variable is time.

<?xml version = '1.0' ?>

<model>

<title><basic> Low-Pass Filter </basic></title>

<structure>
<name> Model </name>

<variables>

We define a low-frequency sinusoid (Signal)
and a high-frequency sinusoid (Noise).

<parm>
  <name> SigAmp </name>
  <val> 1 </val>
</parm>

<parm>
  <name> SigFreq </name>
  <val> 1 </val>
</parm>

<parm>
  <name> NoiseAmp </name>
  <val> 0.5 </val>
</parm>

<parm>
  <name> NoiseFreq </name>
  <val> 5 </val>
</parm>

The cutoff frequency of the filter defines
the filter's performance.  Signals with
frequencies less than the cutoff frequency
will pass through the filter while signals
with higher frequencies will be attenuated.

<parm>
  <name> CutoffFreq </name>
  <val> 2 </val>
</parm>

<var>
  <name> Signal </name>
</var>

<var>
  <name> Noise </name>
</var>

<var>
  <name> Input </name>
</var>

<var>
  <name> K </name>
</var>

</variables>

<equations>

<diffeq>
  <name> Output </name>
  <integralname> Output </integralname>
  <initialval> 0.0 </initialval>
  <dervname> dOutput/dt </dervname>
  <errorlim> 0.01 </errorlim>
</diffeq>

</equations>

<definitions> ===============================

<block><name> Dervs </name> =================

<def>
  <name> Signal </name>
  <val>  
     SigAmp * SIN ( 2 * System.Pi * SigFreq
     * System.X )
  </val>
</def>

<def>
  <name> Noise </name>
  <val>  
     NoiseAmp * SIN ( 2 * System.Pi * NoiseFreq
     * System.X )
  </val>
</def>

The two sinusoids are combined to form the
filter's input.

<def>
  <name> Input </name>
  <val> Signal + Noise </val>
</def>

Now the filter

<def>
  <name> K </name>
  <val> 2 * System.Pi * CutoffFreq </val>
</def>

<def>
  <name> dOutput/dt </name>
  <val> K * ( Input - Output ) </val>
</def>

</block>

</definitions>
</structure>

<math> ========================================

  <dervs> Model.Dervs </dervs>

</math>

<control> =====================================

<gofor>
  <solutionint> 2 </solutionint>
  <displayint> 0.02 </displayint>
</gofor>

</control>

<display> ======================================

<panel><name> Low-Pass Filter </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<infobutton>
  <row> 2.2 </row><col> 1 </col>
  <label> Math </label>
  <line> This model is a first-order, </line>
  <line> low-pass filter which is </line>
  <line> implemented by the first-order </line>
  <line> differential equation </line>
  <line>  </line>
  <line> dOutput / dt = K * (Input - Output) </line>
  <line>  </line>
  <line> The independent variable is time (t). </line>
</infobutton>

<repeatlist>
  <name> Amps </name>
  <repeat>
    <reps> 20 </reps>
    <stepsize> 0.1 </stepsize>
  </repeat>
</repeatlist>

<repeatlist>
  <name> Freqs </name>
  <repeat>
    <reps> 20 </reps>
    <stepsize> 0.1 </stepsize>
  </repeat>
  <repeat>
    <reps> 8 </reps>
    <stepsize> 1.0 </stepsize>
  </repeat>
</repeatlist>

Signal

<groupbox>
   <row> 4 </row><col> 1 </col>
   <high> 4.5 </high><wide> 24 </wide>
   <title> Signal </title>

<slidebar>
  <row> 1.4 </row><col> 1 </col>
  <name> SigAmp </name>
  <listname> Amps </listname>
  <label> Size </label>
</slidebar>

<slidebar>
  <row> 2.9</row><col> 1 </col>
  <name> SigFreq </name>
  <listname> Freqs </listname>
  <label> Hz </label>
</slidebar>

</groupbox>

Noise

<groupbox>
   <row> 10 </row><col> 1 </col>
   <high> 4.5 </high><wide> 24 </wide>
   <title> Noise </title>

<slidebar>
  <row> 1.4 </row><col> 1 </col>
  <name> NoiseAmp </name>
  <listname> Amps </listname>
  <label> Size </label>
</slidebar>

<slidebar>
  <row> 2.9 </row><col> 1 </col>
  <name> NoiseFreq </name>
  <listname> Freqs </listname>
  <label> Hz </label>
</slidebar>

</groupbox>

Cutoff

<groupbox>
   <row> 16 </row><col> 1 </col>
   <high> 3.0 </high><wide> 24 </wide>
   <title> Cutoff </title>

<slidebar>
  <row> 1.4 </row><col> 1 </col>
  <name> CutoffFreq </name>
  <listname> Freqs </listname>
  <label> Hz </label>
</slidebar>

</groupbox>

Right Side ==================================

<showgraph>
  <row> 0.5 </row><col> 27 </col>
  <high> 4.7 </high><wide> 36 </wide>
  <leftmargin> 6 </leftmargin>
  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 2 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> Signal </name>
      <label> Signal </label>
      <linecolor> BLACK </linecolor>
    </yvar>
    <scale><min> -1 </min><max> 1 </max><inc> 1 </inc></scale>
  </yaxis>
</showgraph>

<showgraph>
  <row> 6.1 </row><col> 27 </col>
  <high> 4.7 </high><wide> 36 </wide>
  <leftmargin> 6 </leftmargin>
  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 2 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> Noise </name>
      <label> Noise </label>
      <linecolor> MAGENTA </linecolor>
    </yvar>
    <scale><min> -1 </min><max> 1 </max><inc> 1 </inc></scale>
  </yaxis>
</showgraph>

<showgraph>
  <row> 11.2 </row><col> 27 </col>
  <high> 4.7 </high><wide> 36 </wide>
  <leftmargin> 6 </leftmargin>
  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 2 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> Input </name>
      <label> Input </label>
      <linecolor> BLUE </linecolor>
    </yvar>
    <scale><min> -1 </min><max> 1 </max><inc> 1 </inc></scale>
  </yaxis>
</showgraph>

<showgraph>
  <row> 16.3 </row><col> 27 </col>
  <high> 4.7 </high><wide> 36 </wide>
  <leftmargin> 6 </leftmargin>
  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 2 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> Output </name>
      <label> Output </label>
      <linecolor> RED </linecolor>
    </yvar>
    <scale><min> -1 </min><max> 1 </max><inc> 1 </inc></scale>
  </yaxis>
</showgraph>

</panel>
</display>
</model>

End<?xml version = '1.0' ?>

<model>

<title><basic> Delay </basic></title>

<structure>
<name> Model </name>

<variables> ---------------------------------------

<parm><name> k </name><val> 1 </val></parm>
<parm><name> Strength </name><val> 7 </val></parm>
<var><name> Target </name></var>

<var><name> Ppl </name><val> 1022.31 </val></var>1022.31
<var><name> Pbs </name><val> 1029.81 </val></var>1029.81
<var><name> Ccw </name><val>0.15  </val></var>
<var><name> Rcw </name><val> 0.3 </val></var>
<var><name> Cl </name><val> 0.2 </val></var>
<var><name> Raw </name><val> 1.7 </val></var>
<var><name> pa0 </name><val> 1029.81 </val></var>1029.81
<var><name> Pl </name><val> 1023 </val></var>1025

<var><name> T </name>><val> 0 </val></var>
<var><name> Q </name><val> 0 </val></var>
<var><name> InitPpl </name></var>
<var><name> InitPl </name></var>
<var><name> InitVl </name></var>

<var><name> DelPpl </name></var>
<var><name> DelPl </name></var>
<var><name> DelVl </name></var>

<parm><name> RespiratoryRate </name><val> 12 </val></parm>
<var><name> RespiratoryPeriod </name></var>
<var><name> Inhale </name></var>
<var><name> Exhale </name></var>
<var><name> ParmX </name></var>
<var><name> ModX </name></var>
<var><name> ActualVcw </name></var>
<var><name> ActualVl </name></var>
<parm><name> factor </name><val> 1.0 </val></parm>
<var><name> DriverInit </name><val> -3 </val></var>


<var><name> DisplayPl </name></var>
<var><name> DisplayPbs </name></var>
<var><name> DisplayPpl </name></var>
<var><name> Driver </name></var>
<var><name> PrevPeriods </name><val> 0 </val></var>
<var><name> PreviousT </name><val> 0 </val></var>

</variables>

<equations> ----------------------------------

<diffeq>
  <name> Vcw </name>
  <integralname> Vcw </integralname>
  <initialval> 2.250 </initialval>
  <dervname> dVcw </dervname>
  <errorlim> .05 </errorlim>
</diffeq>

<diffeq>
  <name> Vl</name>
  <integralname> Vl </integralname>
  <initialval> 2.300 </initialval>
  <dervname> dVl </dervname>
  <errorlim> 0.05 </errorlim>
</diffeq>


</equations>

<definitions> -----------------------------------

<block>
<name> Parms </name>

<if>
  <test> System.X EQ 0 </test>
  <true> 
	<def><name> PrevPeriods </name><val> 0 </val></def>
  </true>
</if>
<def><name> ParmX </name><val> System.X - T </val></def>

<if>
<test> RespiratoryRate GT 0 </test>
<true>
<def><name> RespiratoryPeriod </name><val> 60 / RespiratoryRate </val></def>
</true>
<false>
<def><name> RespiratoryPeriod </name><val> 10000</val></def>
</false>
</if>
<def><name> Inhale </name><val> 0.4 * RespiratoryPeriod </val></def>
<def><name> Exhale </name><val> 0.6 * RespiratoryPeriod </val></def>

</block>

<block><name> Dervs </name> ====================

    <def><name> ModX </name><val> System.X - ParmX </val></def>

<def><name> Q </name><val> ROUNDDOWN ( ModX / RespiratoryPeriod ) </val></def>
<def><name> T </name><val> System.X - ( PrevPeriods * RespiratoryPeriod )  </val></def>
  <if> <test> T GE RespiratoryPeriod </test>
      <true>
	<def><name> T </name><val> T - RespiratoryPeriod </val></def>
	<def><name> PrevPeriods </name><val> PrevPeriods + 1 </val></def>
     </true>
  </if>

<if>
 <test> T LE Inhale </test>
  <true>
  <def><name> Driver </name><val> Strength * SIN ( T * 3.1415926 / (  2 * Inhale ) ) </val></def>
 </true>
 <false>
  <def><name> Driver </name><val> Strength * SIN ( ( T + ( 2 * Inhale ) ) * 3.1415926 / ( 2 * Exhale ) ) + Strength </val></def>
 </false>
</if>



 <def><name> pa0 </name><val> ( factor * Pbs ) + ( ( 1 - factor ) * Pl ) </val></def>

<def><name> dVcw </name><val> ( Driver - ( ( Vcw - 2.250 ) / Ccw ) - ( ( Vl - 2.250 ) / Cl ) ) / ( Rcw + Raw )  </val></def>
<def><name> dVl </name><val> dVcw </val></def>
<def><name> Ppl </name><val> pa0 - Driver + ( ( Vcw - 2.250 ) / Ccw ) + ( Rcw * dVcw )  - 5 </val></def>   
<def><name> Pl </name><val> pa0 -  ( Raw * dVcw )  </val></def>


<def><name> DisplayPl </name><val> ( Pl / 1.36 ) - 757.2 </val></def>
<def><name> DisplayPbs </name><val> ( Pbs / 1.36 ) - 757.2 </val></def>
<def><name> DisplayPpl </name><val> ( Ppl / 1.36 ) - 757.2 </val></def>

</block>

</definitions>

</structure>

<math>
  <parms> Model.Parms </parms>
  <dervs> Model.Dervs </dervs>
</math>

<control> ========================================

<gofor>
  <solutionint> 1 </solutionint>
  <displayint> 0.1 </displayint>
</gofor>

<gofor>
  <solutionint> 10 </solutionint>
  <displayint> 0.1 </displayint>
</gofor>

<settlingtime> 10 </settlingtime>

</control>

<display> =========================================

<panel><name>  </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<repeatlist>
  <name> Rate </name>
  <firstval> 0 </firstval>
  <repeat><reps> 20 </reps><stepsize> 0.05 </stepsize></repeat>
</repeatlist>

<repeatlist>
  <name> str </name>
  <repeat><reps> 20 </reps><stepsize> 1 </stepsize></repeat>
</repeatlist>

<editbox>
  <row> 2.4 </row><col> 1.0 </col>
  <name> RespiratoryRate </name>
  <label> Respiration Rate </label>
</editbox>

<slidebar>
  <row> 2.4 </row><col> 40 </col>
  <name> Strength </name>
  <listname> str </listname>
  <label> Str </label>
</slidebar>


<showvalue>
  <row> 1.0 </row><col> 61 </col>
  <name> ParmX </name>
  <format><decimal> 2 </decimal></format>
  <label> ParmX </label>
</showvalue>

<showvalue>
  <row> 2. </row><col> 61 </col>
  <name> T </name>
  <format><decimal> 2 </decimal></format>
  <label> T </label>
</showvalue>

<showvalue>
  <row> 3. </row><col> 61 </col>
  <name> PrevPeriods </name>
  <format><decimal> 2 </decimal></format>
  <label> PrevPeriod </label>
</showvalue>

<showgraph>
  <row> 32.0 </row><col> 1 </col>
  <high> 12 </high><wide> 30 </wide>
  <leftmargin> 5 </leftmargin>
  <xaxis>
    <name> System.X </name>
    <label> time </label>
    <scale><min> 0 </min><max> 10 </max></scale>
  </xaxis>
  <yaxis>
   <yvar>
      <name> DisplayPpl </name>
      <label> Ppl </label>
      <linecolor> BLACK </linecolor>
    </yvar>
    <scale><min> -7 </min><max> -4 </max></scale>

  </yaxis>
</showgraph>

<showgraph>
  <row> 4.0 </row><col> 1 </col>
  <high> 12 </high><wide> 30 </wide>
  <leftmargin> 5 </leftmargin>
  <xaxis>
    <name> System.X </name>
    <label> time </label>
    <scale><min> 0 </min><max> 10 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> Vl </name>
      <label> Vl </label>
      <linecolor> BLACK </linecolor>
    </yvar>
    <scale><min> 2.5 </min><max> 3.25 </max></scale>
  </yaxis>
</showgraph>

<showgraph>
  <row> 18.0 </row><col> 1 </col>
  <high> 12 </high><wide> 30 </wide>
  <leftmargin> 5 </leftmargin>
  <xaxis>
    <name> System.X </name>
    <label> time </label>
    <scale><min> 0 </min><max> 10 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> DisplayPl </name>
      <label> Pl </label>
      <linecolor> BLACK </linecolor>
    </yvar>
        <scale><min> 0 </min><max> 1.0 </max></scale>
  </yaxis>
</showgraph>

<showgraph>
  <row> 4.0 </row><col> 41 </col>
  <high> 12 </high><wide> 30 </wide>
  <leftmargin> 5 </leftmargin>
  <xaxis>
    <name> System.X </name>
    <label> time </label>
    <scale><min> 0 </min><max> 10 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> Driver </name>
      <label> Driver </label>
      <linecolor> BLACK </linecolor>
    </yvar>
    <scale><min> 0 </min><max> 1 </max></scale>
  </yaxis>
</showgraph>

<showgraph>
  <row> 18.0 </row><col> 41 </col>
  <high> 12 </high><wide> 30 </wide>
  <leftmargin> 5 </leftmargin>
  <xaxis>
    <name> System.X </name>
    <label> time </label>
    <scale><min> 0 </min><max> 10 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> T </name>
      <label> T </label>
      <linecolor> BLACK </linecolor>
    </yvar>
   <yvar>
      <name> Inhale </name>
      <label> Inhale </label>
      <linecolor> BLUE </linecolor>
    </yvar>

    <scale><min> 0 </min><max> 1 </max></scale>
  </yaxis>
</showgraph>


</panel>
</display>

</model>

End  Michaelis-Menten

Created : 29-Dec-03
Last Modified : 01-Dec-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

<?xml version = '1.0' ?>

<model>

<title><basic> Michaelis-Menten </basic></title>

<structure><name> Model </name>

<variables> =================================

<var>
  <name> VelocityOne-Two </name>
</var>

<var>
  <name> VelocityOne-Three </name>
</var>

<var>
  <name> [S]One </name>
</var>

<var>
  <name> [S]Two </name>
</var>

<var>
  <name> [S]Three </name>
</var>

<parm>
  <name> Formation </name>
  <val> 4.6 </val>
</parm>

<parm>
  <name> Volume </name>
  <val> 1.0 </val>
</parm>

<parm>
  <name> KTwo </name>
  <val> 1.0 </val>
</parm>

<parm>
  <name> KThree </name>
  <val> 1.0 </val>
</parm>

</variables>

<equations>

<diffeq>
  <name> MassOne </name>
  <integralname> MassOne </integralname>
  <initialval> 1.0 </initialval>
  <dervname> ChangeOne </dervname>
  <errorlim> 0.01 </errorlim>
</diffeq>

<diffeq>
  <name> MassTwo </name>
  <integralname> MassTwo </integralname>
  <initialval> 1.3 </initialval>
  <dervname> ChangeTwo </dervname>
  <errorlim> 0.01 </errorlim>
</diffeq>

<diffeq>
  <name> MassThree </name>
  <integralname> MassThree </integralname>
  <initialval> 3.3 </initialval>
  <dervname> ChangeThree </dervname>
  <errorlim> 0.01 </errorlim>
</diffeq>

</equations>

<definitions> =====================================

<block><name> Dervs </name> =======================

<def>
  <name> [S]One </name>
  <val> MassOne / Volume </val>
</def>

<def>
  <name> [S]Two </name>
  <val> MassTwo / Volume </val>
</def>

<def>
  <name> [S]Three </name>
  <val> MassThree / Volume </val>
</def>

<def>
  <name> VelocityOne-Two </name>
  <val> ( 2.0 * [S]One ) / ( 0.5 + [S]One ) </val>
</def>

<def>
  <name> VelocityOne-Three </name>
  <val> ( 10.0 * [S]One ) / ( 2.0 + [S]One ) </val>
</def>

<def>
  <name> ChangeOne </name>
  <val> Formation - VelocityOne-Two - VelocityOne-Three </val>
</def>

<def>
  <name> ChangeTwo </name>
  <val> VelocityOne-Two - ( KTwo * MassTwo ) </val>
</def>

<def>
  <name> ChangeThree </name>
  <val> VelocityOne-Three - ( KThree * MassThree ) </val>
</def>

</block>

</definitions>
</structure>

<math> ==========================================

  <dervs> Model.Dervs </dervs>

</math>

<control> ========================================


<gofor>
  <solutionint> 10 </solutionint>
  <displayint> 1 </displayint>
  <menuitem> 10 Sec </menuitem>
</gofor>


</control>

<display> =========================================

<panel><name> Michaelis-Menten </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<showclock>
   <row> 0.5 </row><col> 36 </col>
   <name> System.X </name>
   <timebase> SEC </timebase>
</showclock>

<showvalue>
   <row> 2 </row><col> 1 </col>
   <name> [S]One </name>
   <format><decimal> 1 </decimal></format>
   <label> [S1] </label>
</showvalue>

<showgraph>
  <row> 3.0 </row><col> 1 </col>
  <high> 7 </high><wide> 28 </wide>
  <leftmargin> 7 </leftmargin>
  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 10 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> [S]One </name>
      <label> [S1] </label>
    </yvar>
    <scale><min> 0 </min><max> 2 </max><inc> 1 </inc></scale>
  </yaxis>
</showgraph>

<showvalue>
   <row> 10.4 </row><col> 1 </col>
   <name> MassOne </name>
   <format><decimal> 1 </decimal></format>
   <label> Mass 1 </label>
</showvalue>

<showvalue>
   <row> 11.4 </row><col> 3 </col>
   <name> ChangeOne </name>
   <format><decimal> 1 </decimal></format>
   <label> Change 1 </label>
</showvalue>

<editbox>
   <row> 12.8 </row><col> 3 </col>
   <name> Formation </name>
   <label> Formation </label>
</editbox>

<showvalue>
   <row> 14.2 </row><col> 3 </col>
   <name> VelocityOne-Two </name>
   <format><decimal> 1 </decimal></format>
   <label> Velocity 1-2 </label>
</showvalue>

<showvalue>
   <row> 15.2 </row><col> 3 </col>
   <name> VelocityOne-Three </name>
   <format><decimal> 1 </decimal></format>
   <label> Velocity 1-3 </label>
</showvalue>

<showvalue>
   <row> 17.0 </row><col> 1 </col>
   <name> [S]Two </name>
   <format><decimal> 1 </decimal></format>
   <label> [S2] </label>
</showvalue>

<showgraph>
  <row> 18.0 </row><col> 1 </col>
  <high> 7 </high><wide> 28 </wide>
  <leftmargin> 7 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 10 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> [S]Two </name>
      <label> [S2] </label>
    </yvar>
    <scale><min> 0 </min><max> 2 </max><inc> 1 </inc></scale>
  </yaxis>
</showgraph>

<showvalue>
   <row> 25.4 </row><col> 1 </col>
   <name> MassTwo </name>
   <format><decimal> 1 </decimal></format>
   <label> Mass 2 </label>
</showvalue>

<showvalue>
   <row> 26.4 </row><col> 3 </col>
   <name> ChangeTwo </name>
   <format><decimal> 1 </decimal></format>
   <label> Change 2 </label>
</showvalue>

<showvalue>
   <row> 17.0 </row><col> 32 </col>
   <name> [S]Three </name>
   <format><decimal> 1 </decimal></format>
   <label> [S3] </label>
</showvalue>

<showgraph>
  <row> 18.0 </row><col> 32 </col>
  <high> 7 </high><wide> 28 </wide>
  <leftmargin> 7 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 10 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> [S]Three </name>
      <label> [S3] </label>
    </yvar>
    <scale><min> 0 </min><max> 2 </max><inc> 1 </inc></scale>
  </yaxis>
</showgraph>

<showvalue>
   <row> 25.4 </row><col> 32 </col>
   <name> MassThree </name>
   <format><decimal> 1 </decimal></format>
   <label> Mass 3 </label>
</showvalue>

<showvalue>
   <row> 26.4 </row><col> 34 </col>
   <name> ChangeThree </name>
   <format><decimal> 1 </decimal></format>
   <label> Change 3 </label>
</showvalue>

</panel>
</display>

</model>

End
Nerve

Created : 25-Aug-96
Last Modified : 02-Dec-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

The Hodgkin-Huxley equations describe
the genesis of an action potential.
Membrane potential is a function of
the charge separation across the
membrane and membrane capacitance.
Charge separation is the integral
over time of current flowing through
the membrane.  Current is the sum of
sodium, potassium and leakage (other
ions) ion currents.  Each of these
currents is a function of membrane
potential and ion conductances.  The
conductances are voltage sensitive.

The independent variable is time in
milliseconds.

Membrane charge is the integral over
time of membrane current.  Membrane
potential is a function of membrane
charge and membrane capacitance.

<?xml version = '1.0' ?>

<model>

<title><basic> Hodgkin-Huxley </basic></title>

<structure>
<name> Model </name>

<variables> ===============================

<parm><name> C </name><val> 1 </val></parm>
<parm><name> NaTotal </name><val> 120 </val></parm>

Stimulators add to the membrane current.

<var><name> Switch </name></var>
<var><name> Time </name></var>
<var><name> Duration </name></var>
<var><name> Amplitude </name></var>
<var><name> Output </name></var>

<parm>
  <name> StimSwitch1 </name>
  <val> 0 </val>
</parm>

<parm>
  <name> FireAt1 </name>
  <val> 4 </val>
</parm>

<parm>
  <name> Duration1 </name>
  <val> 0.3 </val>
</parm>

<var>
  <name> OldState1 </name>
  <val> 0 </val>
</var>

<parm>
  <name> StimCurrent1 </name>
  <val> -300 </val>
</parm>

<parm>
  <name> StimSwitch2 </name>
  <val> 0 </val>
</parm>

<parm>
  <name> FireAt2 </name>
  <val> 8 </val>
</parm>

<parm>
  <name> Duration2 </name>
  <val> 0.3 </val>
</parm>

<var>
  <name> OldState2 </name>
  <val> 0 </val>
</var>

<parm>
  <name> StimCurrent2 </name>
  <val> -300 </val>
</parm>

Equilibration potential and leak.

<parm>
  <name> VNa </name>
  <val> 50 </val>
</parm>

<parm>
  <name> VK </name>
  <val> -75 </val>
</parm>

<parm>
  <name> GLeak </name>
  <val> 0.3 </val>
</parm>

<parm>
  <name> VLeak </name>
  <val> -50 </val>
</parm>

<var>
  <name> V </name>
</var>

<var>
  <name> NaActiveSS </name>
</var>

<var>
  <name> NaInactive </name>
</var>

<var>
  <name> NaActiveTau </name>
</var>

<var>
  <name> FractSS </name>
</var>

<var>
  <name> NaOpen </name>
</var>

<var>
  <name> NaClosed </name>
</var>

<var>
  <name> FractTau </name>
</var>

<var>
  <name> GNa </name>
</var>

<var>
  <name> GKSS </name>
</var>

<var>
  <name> GKTau </name>
</var>

<var>
  <name> IStim1 </name>
</var>

<var>
  <name> IStim2 </name>
</var>

<var>
  <name> INa </name>
</var>

<var>
  <name> IK </name>
</var>

<var>
  <name> ILeak </name>
</var>

</variables>

<equations> ================================

<diffeq>
  <name> Q </name>
  <integralname> Q </integralname>
  <initialval> 75.0 </initialval>
  <dervname> dQ/dt </dervname>
  <errorlim> 1.0 </errorlim>
</diffeq>

Active Na+ channel pool.

<diffeq>
  <name> NaActive </name>
  <integralname> NaActive </integralname>
  <initialval> 90.0 </initialval>
  <dervname> dNaActive/dt </dervname>
  <errorlim> 1.0 </errorlim>
</diffeq>

<diffeq>
  <name> Fract </name>
  <integralname> Fract </integralname>
  <initialval> 0.0 </initialval>
  <dervname> dFract/dt </dervname>
  <errorlim> 0.01 </errorlim>
</diffeq>

<diffeq>
  <name> GK </name>
  <integralname> GK </integralname>
  <initialval> 7.2 </initialval>
  <dervname> dGK/dt </dervname>
  <errorlim> 0.1 </errorlim>
</diffeq>

</equations>

<functions> ==================================

<curve>
  <name> NaActiveTau </name>
  <point>
    <x> -100 </x><y>  2 </y><slope> 0.1 </slope>
  </point>
  <point>
    <x>  -50 </x><y> 10 </y><slope> 0 </slope>
  </point>
  <point>
    <x>   30 </x><y> 2 </y><slope> 0 </slope>
  </point>
</curve>

<curve>
  <name> PoolSS </name>
  <point>
    <x> -90 </x><y> 1 </y><slope> 0 </slope>
  </point>
  <point>
    <x> -30 </x><y> 0 </y><slope> 0 </slope>
  </point>
</curve>

<curve>
  <name> FractSS </name>
  <point>
    <x> -60 </x><y> 0 </y><slope> 0 </slope>
  </point>
  <point>
    <x>  25 </x><y> 1 </y><slope> 0 </slope>
  </point>
</curve>

<curve>
  <name> FractTau </name>
  <point>
    <x> -100 </x><y> 0.15 </y><slope> 0.004 </slope>
  </point>
  <point>
    <x>  -40 </x><y> 0.40 </y><slope> 0 </slope>
  </point>
  <point>
    <x>   50 </x><y> 0.10 </y><slope> 0 </slope>
  </point>
</curve>

<curve>
  <name> GKSS </name>
  <point>
    <x> -100 </x><y>  3.6 </y><slope> 0 </slope>
  </point>
  <point>
    <x>   25 </x><y> 36.0 </y><slope> 0 </slope>
  </point>
</curve>

<curve>
  <name> GKTau </name>
  <point>
    <x> -100 </x><y> 4 </y><slope> 0.02 </slope>
  </point>
  <point>
    <x>  -50 </x><y> 6 </y><slope> 0 </slope>
  </point>
  <point>
    <x>   30 </x><y> 1 </y><slope> 0 </slope>
  </point>
</curve>

</functions>

<definitions> ================================

<block><name> Dervs </name> ==================

<def>
  <name> V </name>
  <val> ( -1 / C ) * Q </val>
</def>

Sodium channels can be active or
inactive.  Active channels may be
open or closed.  Open channels
determine the sodium conductance.
Inactive channels are closed.

The size of the pool of active
channels and the transition delay
between active and inactive states
are both voltage dependent.

If all sodium channels are active
and open, conductance will be 120.

<def>
  <name> NaActiveSS </name>
  <val> NaTotal * PoolSS [ V ] </val>
</def>

<def>
  <name> NaInactive </name>
  <val> NaTotal - NaActive </val>
</def>

<def>
  <name> NaActiveTau </name>
  <val> NaActiveTau [ V ] </val>
</def>

<def>
  <name> dNaActive/dt </name>
  <val>
      ( 1 / NaActiveTau )
    * ( NaActiveSS - NaActive )
  </val>
</def>

The fraction of active sodium channels
that are open and the transition delay
between open and closed are both
voltage dependent.

Finally, we have enough information to
calculate the sodium conductance.

<def>
  <name> FractSS </name>
  <val> FractSS [ V ] </val>
</def>

<def>
  <name> NaOpen </name>
  <val> Fract * NaActive </val>
</def>

<def>
  <name> NaClosed </name>
  <val> NaTotal - NaOpen </val>
</def>

<def>
  <name> FractTau </name>
  <val> FractTau [ V ] </val>
</def>

<def>
  <name> dFract/dt </name>
  <val> 
      ( 1 / FractTau )
    * ( FractSS - Fract )
  </val>
</def>

<def>
  <name> GNa </name>
  <val> Fract * NaActive </val>
</def>

Potassium conductance is determined
by open potassium channels.  Opening
is voltage dependent and there is a
voltage sensitive delay in the
transition.

<def>
  <name> GKSS </name>
  <val> GKSS [ V ] </val>
</def>

<def>
  <name> GKTau </name>
  <val> GKTau [ V ] </val>
</def>

<def>
  <name> dGK/dt </name>
  <val> ( 1 / GKTau ) * ( GKSS - GK ) </val>
</def>

Stimulators add to the membrane current.

<copy><from> StimSwitch1 </from><to> Switch </to></copy>
<copy><from> FireAt1 </from><to> Time </to></copy>
<copy><from> Duration1 </from><to> Duration </to></copy>
<copy><from> StimCurrent1 </from><to> Amplitude </to></copy>
<call> Stimulator </call>
<copy><from> Output </from><to> IStim1 </to></copy>

<copy><from> StimSwitch2 </from><to> Switch </to></copy>
<copy><from> FireAt2 </from><to> Time </to></copy>
<copy><from> Duration2 </from><to> Duration </to></copy>
<copy><from> StimCurrent2 </from><to> Amplitude </to></copy>
<call> Stimulator </call>
<copy><from> Output </from><to> IStim2 </to></copy>

Equilibration potentials are a
function of intra- and extracellular
ion concentrations.  These potentials
are parameters here.  Ion currents
are a function of equilibrium
potentials and conductance.

<def>
  <name> INa </name>
  <val> GNa * ( V - VNa ) </val>
</def>

<def>
  <name> IK </name>
  <val> GK * ( V - VK ) </val>
</def>

<def>
  <name> ILeak </name>
  <val> GLeak * ( V - VLeak ) </val>
</def>

<def>
  <name> dQ/dt </name>
  <val> INa + IK + ILeak + IStim1 + IStim2 </val>
</def>

</block>

<block><name> Stimulator </name>

<conditional>
  <name> Output </name>
  <test>
        Switch
    AND ( System.X GE Time )
    AND ( System.X LE ( Time + Duration ) )
  </test>
  <true> Amplitude </true>
  <false> 0.0 </false>
</conditional>

</block>

</definitions>
</structure>

<math> ==========================================

  <dervs> Model.Dervs </dervs>

</math>

<control> ========================================

<gofor>
  <solutionint> 1 </solutionint>
  <displayint> 0.5 </displayint>
  <menuitem> 1 mS </menuitem>
</gofor>

<gofor>
  <solutionint> 5 </solutionint>
  <displayint> 0.5 </displayint>
  <menuitem> 5 mS </menuitem>
</gofor>

<gofor>
  <solutionint> 10 </solutionint>
  <displayint> 0.5 </displayint>
  <menuitem> 10 mS </menuitem>
</gofor>

</control>

<display> ====================================

<common>

<maplist>
  <name> Model.Switch </name>
  <map><val> 0 </val><img> Off </img></map>
  <map><val> 1 </val><img> On </img></map>
</maplist>

</common>

<panel><name> Hodgkin-Huxley </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<showvalue>
   <row> 2.5 </row><col> 1 </col>
   <name> V </name>
   <format> <integer/> </format>
   <label> Membrane Potential ( mV ) </label>
</showvalue>

<showvalue>
   <row> 4.5 </row><col> 1 </col>
   <name> INa </name>
   <format> <integer/> </format>
   <label> Sodium Current </label>
</showvalue>

<showvalue>
   <row> 5.5 </row><col> 1 </col>
   <name> IK </name>
   <format> <integer/> </format>
   <label> Potassium Current </label>
</showvalue>

<showvalue>
   <row> 6.5 </row><col> 1 </col>
   <name> ILeak </name>
   <format> <integer/> </format>
   <label> Leak Current </label>
</showvalue>

<showvalue>
   <row> 8.5 </row><col> 1 </col>
   <name> GNa </name>
   <format><decimal> 1 </decimal></format>
   <label> Sodium Conductance </label>
</showvalue>

<showvalue>
   <row> 9.5 </row><col> 1 </col>
   <name> GK </name>
   <format><decimal> 1 </decimal></format>
   <label> Potassium Conductance </label>
</showvalue>

<showvalue>
   <row> 10.5 </row><col> 1 </col>
   <name> GLeak </name>
   <format><decimal> 1 </decimal></format>
   <label> Leak Conductance </label>
</showvalue>

Some lists for the stimulators are
next.

<repeatlist>
  <name> Current </name>
  <firstval> -500 </firstval>
  <repeat>
    <reps> 20 </reps>
    <stepsize> 50 </stepsize>
  </repeat>
</repeatlist>

<repeatlist>
  <name> FireAt </name>
  <repeat>
    <reps> 20 </reps>
    <stepsize> 1 </stepsize>
  </repeat>
</repeatlist>

<repeatlist>
  <name> Duration </name>
  <repeat>
    <reps> 20 </reps>
    <stepsize> 0.1 </stepsize>
  </repeat>
</repeatlist>

Stimulator 1

<groupbox>
   <row> 12 </row><col> 1 </col>
   <high> 8.4 </high><wide> 26 </wide>
   <title> Stimulator 1 </title>

<radiobuttons>
  <row> 1.4 </row><col> 1 </col>
  <name> StimSwitch1 </name>
  <listname> Switch </listname>
  <label> Switch </label>
</radiobuttons>

<slidebar>
  <row> 2.8 </row><col> 1 </col>
  <name> StimCurrent1 </name>
  <listname> Current </listname>
  <label> Current </label>
</slidebar>

<slidebar>
  <row> 4.2 </row><col> 1 </col>
  <name> FireAt1 </name>
  <listname> FireAt </listname>
  <label> Fire At </label>
</slidebar>

<slidebar>
  <row> 5.6 </row><col> 1 </col>
  <name> Duration1 </name>
  <listname> Duration </listname>
  <label> Duration </label>
</slidebar>

<showvalue>
   <row> 7.0 </row><col> 1 </col>
   <name> IStim1 </name>
   <format> <integer/> </format>
   <label> Output </label>
</showvalue>

</groupbox>

Stimulator 2

<groupbox>
   <row> 21 </row><col> 1 </col>
   <high> 8.4 </high><wide> 26 </wide>
   <title> Stimulator 2 </title>

<radiobuttons>
  <row> 1.4 </row><col> 1 </col>
  <name> StimSwitch2 </name>
  <listname> Switch </listname>
  <label> Switch </label>
</radiobuttons>

<slidebar>
  <row> 2.8 </row><col> 1 </col>
  <name> StimCurrent2 </name>
  <listname> Current </listname>
  <label> Current </label>
</slidebar>

<slidebar>
  <row> 4.2 </row><col> 1 </col>
  <name> FireAt2 </name>
  <listname> FireAt </listname>
  <label> Fire At </label>
</slidebar>

<slidebar>
  <row> 5.6 </row><col> 1 </col>
  <name> Duration2 </name>
  <listname> Duration </listname>
  <label> Duration </label>
</slidebar>

<showvalue>
   <row> 7.0 </row><col> 1 </col>
   <name> IStim2 </name>
   <format> <integer/> </format>
   <label> Output </label>
</showvalue>

</groupbox>

Na+ Channels

<groupbox>
   <row> 30 </row><col> 1 </col>
   <high> 9.1 </high><wide> 26 </wide>
   <title> Na+ Channels </title>

<showvalue>
   <row> 1.4 </row><col> 1 </col>
   <name> NaTotal </name>
   <format> <integer/> </format>
   <label> Total </label>
</showvalue>

<showvalue>
   <row> 2.8 </row><col> 1 </col>
   <name> NaInactive </name>
   <format> <integer/> </format>
   <label> Inactive </label>
</showvalue>

<showvalue>
   <row> 3.8 </row><col> 1 </col>
   <name> NaActive </name>
   <format> <integer/> </format>
   <label> Active </label>
</showvalue>

<showvalue>
   <row> 5.2 </row><col> 1 </col>
   <name> NaOpen </name>
   <format> <integer/> </format>
   <label> Open </label>
</showvalue>

<showvalue>
   <row> 6.2 </row><col> 1 </col>
   <name> NaClosed </name>
   <format> <integer/> </format>
   <label> Closed </label>
</showvalue>

<infobutton>
  <row> 7.6 </row><col> 1 </col>
  <label> Init Values </label>
  <line> Initial Na+ Channels </line>
  <line>  </line>
  <line> Total - 120 </line>
  <line>  </line>
  <line> Inactive - 30 </line>
  <line> Active - 90 </line>
  <line>  </line>
  <line> Open - 0 </line>
  <line> Closed - 120 </line>
</infobutton>

</groupbox>

Reference

<infobutton>
  <row> 40.6 </row><col> 1 </col>
  <label> Reference </label>
  <line> Hodgkin, A.L. and A.F. Huxley </line>
  <line> A quantitative description of </line>
  <line> membrane current and its </line>
  <line> application to conduction. </line>
  <line> J. Physiol. 117:500-544, 1952. </line>
</infobutton>

Right Side ====================================

<showvalue>
   <row> 0.5 </row><col> 28 </col>
   <name> System.X </name>
   <format><decimal> 1 </decimal></format>
   <label> Time ( mS ) </label>
</showvalue>

<showgraph>
  <row> 2.5 </row><col> 28 </col>
  <high> 10 </high><wide> 32 </wide>
  <leftmargin> 6 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 20 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> V </name>
      <label> V </label>
      <linecolor> RED </linecolor>
    </yvar>
    <scale><min> -100 </min><max> 40 </max><inc> 20 </inc></scale>
  </yaxis>
</showgraph>

<label>
  <row> 13 </row><col> 28 </col>
  <text> Stimulator Current </text>
</label>

<showgraph>
  <row> 14 </row><col> 28 </col>
  <high> 6 </high><wide> 32 </wide>
  <leftmargin> 6 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 20 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> IStim1 </name>
      <label> 1 </label>
      <linecolor> RED </linecolor>
    </yvar>
    <yvar>
      <name> IStim2 </name>
      <label> 2 </label>
      <linecolor> BLUE </linecolor>
    </yvar>
    <scale><min> -300 </min><max> 0 </max><inc> 100 </inc></scale>
  </yaxis>
</showgraph>

<label>
  <row> 21 </row><col> 28 </col>
  <text> Ion Current </text>
</label>

<showgraph>
  <row> 22 </row><col> 28 </col>
  <high> 8 </high><wide> 32 </wide>
  <leftmargin> 6 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 20 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> INa </name>
      <label> Na </label>
      <linecolor> RED </linecolor>
    </yvar>
    <yvar>
      <name> IK </name>
      <label> K </label>
      <linecolor> BLUE </linecolor>
    </yvar>
    <scale><min> -2500 </min><max> 2500 </max></scale>
  </yaxis>
</showgraph>

<label>
  <row> 31 </row><col> 28 </col>
  <text> Conductance </text>
</label>

<showgraph>
  <row> 32 </row><col> 28 </col>
  <high> 8 </high><wide> 32 </wide>
  <leftmargin> 6 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 20 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> GNa </name>
      <label> Na </label>
      <linecolor> RED </linecolor>
    </yvar>
    <yvar>
      <name> GK </name>
      <label> K </label>
      <linecolor> BLUE </linecolor>
    </yvar>
    <yvar>
      <name> GLeak </name>
      <label> Leak </label>
      <linecolor> BLACK </linecolor>
    </yvar>
    <scale><min> 0 </min><max> 80 </max><inc> 20 </inc></scale>
  </yaxis>
</showgraph>

<label>
  <row> 41 </row><col> 28 </col>
  <text> Na+ Channels </text>
</label>

<showgraph>
  <row> 42 </row><col> 28 </col>
  <high> 7 </high><wide> 32 </wide>
  <leftmargin> 6 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 20 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> NaActive </name>
      <label> Active </label>
      <linecolor> RED </linecolor>
    </yvar>
    <yvar>
      <name> NaOpen </name>
      <label> Open </label>
      <linecolor> BLUE </linecolor>
    </yvar>
    <scale><min> 0 </min><max> 120 </max></scale>
  </yaxis>
</showgraph>

</panel>
</display>

</model>

End
Noise

Created : 23-Aug-96
Last Modified : 01-Dec-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

A low-pass filter will smooth an oscillating
signal by attenuating higher frequency
oscillations while passing lower frequency
oscillations.

This model is a first-order, low-pass filter
which is implemented by the first-order
differential equation

   dOutput / dt = K * (Input - Output)

The independent variable is time.

<?xml version = '1.0' ?>

<model>

<title><basic> Low-Pass Filter </basic></title>

<structure>
<name> Model </name>

<variables>

We define a low-frequency sinusoid (Signal).
Noise is random.

<parm>
  <name> SigAmp </name>
  <val> 1 </val>
</parm>

<parm>
  <name> SigFreq </name>
  <val> 1 </val>
</parm>

<parm>
  <name> NoiseAmp </name>
  <val> 1 </val>
</parm>

The cutoff frequency of the filter defines
the filter's performance.  Signals with
frequencies less than the cutoff frequency
will pass through the filter while signals
with higher frequencies will be attenuated.

<parm>
  <name> CutoffFreq </name>
  <val> 2 </val>
</parm>

<var>
  <name> Signal </name>
</var>

<var>
  <name> Noise </name>
</var>

<var>
  <name> Input </name>
</var>

<var>
  <name> K </name>
</var>

</variables>

<equations>

<diffeq>
  <name> Output </name>
  <integralname> Output </integralname>
  <initialval> 0.0 </initialval>
  <dervname> dOutput/dt </dervname>
  <errorlim> 0.01 </errorlim>
</diffeq>

</equations>

<definitions> ===============================

<block><name> Dervs </name> =================

<def>
  <name> Signal </name>
  <val>  
     SigAmp * SIN ( 2 * System.Pi * SigFreq
     * System.X )
  </val>
</def>

<def>
  <name> Noise </name>
  <val> NoiseAmp * System.Random </val>
</def>

The two signals are combined to form the
filter's input.

<def>
  <name> Input </name>
  <val> Signal + Noise </val>
</def>

Now the filter

<def>
  <name> K </name>
  <val> 2 * System.Pi * CutoffFreq </val>
</def>

<def>
  <name> dOutput/dt </name>
  <val> K * ( Input - Output ) </val>
</def>

</block>

</definitions>
</structure>

<math> ========================================

  <dervs> Model.Dervs </dervs>

</math>

<control> =====================================


<gofor>
  <solutionint> 2 </solutionint>
  <displayint> 0.02 </displayint>
</gofor>


</control>

<display> ======================================

<panel><name> Noise + Filter </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<infobutton>
  <row> 2.2 </row><col> 1 </col>
  <label> Math </label>
  <line> This model is a first-order, </line>
  <line> low-pass filter which is </line>
  <line> implemented by the first-order </line>
  <line> differential equation </line>
  <line>  </line>
  <line> dOutput / dt = K * (Input - Output) </line>
  <line>  </line>
  <line> The independent variable is time (t). </line>
</infobutton>

<repeatlist>
  <name> Amps </name>
  <repeat>
    <reps> 20 </reps>
    <stepsize> 0.1 </stepsize>
  </repeat>
</repeatlist>

<repeatlist>
  <name> Freqs </name>
  <repeat>
    <reps> 20 </reps>
    <stepsize> 0.1 </stepsize>
  </repeat>
  <repeat>
    <reps> 8 </reps>
    <stepsize> 1.0 </stepsize>
  </repeat>
</repeatlist>

Signal

<groupbox>
   <row> 4 </row><col> 1 </col>
   <high> 4.5 </high><wide> 24 </wide>
   <title> Signal </title>

<slidebar>
  <row> 1.4 </row><col> 1 </col>
  <name> SigAmp </name>
  <listname> Amps </listname>
  <label> Size </label>
</slidebar>

<slidebar>
  <row> 2.9</row><col> 1 </col>
  <name> SigFreq </name>
  <listname> Freqs </listname>
  <label> Hz </label>
</slidebar>

</groupbox>

Noise

<groupbox>
   <row> 10 </row><col> 1 </col>
   <high> 3.0 </high><wide> 24 </wide>
   <title> Noise </title>

<slidebar>
  <row> 1.4 </row><col> 1 </col>
  <name> NoiseAmp </name>
  <listname> Amps </listname>
  <label> Size </label>
</slidebar>

</groupbox>

Cutoff

<groupbox>
   <row> 14.5 </row><col> 1 </col>
   <high> 3.0 </high><wide> 24 </wide>
   <title> Cutoff </title>

<slidebar>
  <row> 1.4 </row><col> 1 </col>
  <name> CutoffFreq </name>
  <listname> Freqs </listname>
  <label> Hz </label>
</slidebar>

</groupbox>

Right Side ==================================

<showgraph>
  <row> 0.5 </row><col> 27 </col>
  <high> 4.7 </high><wide> 36 </wide>
  <leftmargin> 6 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 2 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> Signal </name>
      <label> Signal </label>
      <linecolor> BLACK </linecolor>
    </yvar>
    <scale><min> -1 </min><max> 1 </max><inc> 1 </inc>
    </scale>
  </yaxis>
</showgraph>

<showgraph>
  <row> 6.1 </row><col> 27 </col>
  <high> 4.7 </high><wide> 36 </wide>
  <leftmargin> 6 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 2 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> Noise </name>
      <label> Noise </label>
      <linecolor> MAGENTA </linecolor>
    </yvar>
    <scale><min> -1 </min><max> 1 </max><inc> 1 </inc></scale>
  </yaxis>
</showgraph>

<showgraph>
  <row> 11.2 </row><col> 27 </col>
  <high> 4.7 </high><wide> 36 </wide>
  <leftmargin> 6 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 2 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> Input </name>
      <label> Input </label>
      <linecolor> BLUE </linecolor>
    </yvar>
    <scale><min> -1 </min><max> 1 </max><inc> 1 </inc></scale>
  </yaxis>
</showgraph>

<showgraph>
  <row> 16.3 </row><col> 27 </col>
  <high> 4.7 </high><wide> 36 </wide>
  <leftmargin> 6 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 2 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> Output </name>
      <label> Output </label>
      <linecolor> RED </linecolor>
    </yvar>
    <scale><min> -1 </min><max> 1 </max><inc> 1 </inc></scale>
  </yaxis>
</showgraph>

</panel>
</display>
</model>

EndP Stewart pH

Created : 22-Aug-96
Last Modified : 01-Dec-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

<?xml version = '1.0' ?>

<model>

<title><basic> Peter Stewart pH </basic></title>

<structure>
<name> Model </name>

<variables> ========================================

Values represent plasma at 37C.  Units
are Eq / L.

Water dissociation

   [H+] * [OH-] = K'w
   K'w = 4.4e-14

<parm>
  <name> K'w </name>
  <val> 4.4e-14 </val>
</parm>

Weak acid dissociation

   [H+] * [A-] = Ka * [HA]
   Ka = 2.0e-7

<parm>
  <name> Ka </name>
  <val> 2.0e-7 </val>
</parm>

Weak acid conservation

   [HA] + [A-] = [ATot]

Bicarbonate formation

   [H+] * [HCO3-] = Kc * pCO2
   Kc = 2.58e-11

<parm>
  <name> Kc </name>
  <val> 2.58e-11 </val>
</parm>

Carbonate formation

   [H+] * [CO3--] = K3 * [HCO3-]
   K3 = 6.0e-11

<parm>
  <name> K3 </name>
  <val> 6.0e-11 </val>
</parm>

Electrical neutrality

   [SID] + [H+] - [HCO3-] - [A-] - [CO3--] - [OH-] = 0

Now some inputs.

<parm>
  <name> ATotmEq </name>
  <val> 20 </val>
</parm>

<parm>
  <name> pCO2 </name>
  <val> 45.0 </val>
</parm>

<parm>
  <name> SIDmEq </name>
  <val> 42 </val>
</parm>

<var>
  <name> OH- </name>
</var>

<var>
  <name> ATot </name>
</var>

<var>
  <name> A- </name>
</var>

<var>
  <name> HCO3- </name>
</var>

<var>
  <name> CO3-- </name>
</var>

<var>
  <name> SID </name>
</var>

<var>
  <name> CO3Eq </name>
</var>

<var>
  <name> HA </name>
</var>

<var>
  <name> Sc </name>
</var>

<var>
  <name> CO2 </name>
</var>

<var>
  <name> K'c </name>
</var>

<var>
  <name> H2CO3 </name>
</var>

<var>
  <name> HIon </name>
</var>

<var>
  <name> pH </name>
</var>

<var>
  <name> OHIon </name>
</var>

<var>
  <name> A-mEq </name>
</var>

<var>
  <name> HAmEq </name>
</var>

<var>
  <name> CO2mMol </name>
</var>

<var>
  <name> CO2Gas </name>
</var>

<var>
  <name> H2CO3uMol </name>
</var>

<var>
  <name> HCO3-mEq </name>
</var>

<var>
  <name> CO3--uMol </name>
</var>

<var>
  <name> CO3--uEq </name>
</var>

</variables>

<equations> =====================================

The rate constants and conditions of
electric neutrality lead to a big
implicit algebraic equation.

<impliciteq>
  <name> H </name>
  <startname> H </startname>
  <initialval> 4.2e-8 </initialval>
  <endname> HEnd </endname>
  <errorlim> 4e-10 </errorlim>
  <searchminname> SearchMin </searchminname>
</impliciteq>

</equations>

<definitions> =====================================

<block><name> Dervs </name> =======================

<def>
  <name> SearchMin </name>
  <val> 1.0e-50 </val>
</def>

Model starts here with H+ = f(H+)

<implicitmath><name> H </name>

<def>
  <name> OH- </name>
  <val> K'w / H </val>
</def>

<def>
  <name> ATot </name>
  <val> 0.001 * ATotmEq </val>
</def>

<def>
  <name> A- </name>
  <val> ( Ka * ATot ) / ( H + Ka ) </val>
</def>

<def>
  <name> HCO3- </name>
  <val> ( Kc * pCO2 ) / H </val>
</def>

<def>
  <name> CO3-- </name>
  <val> ( K3 * HCO3- ) / H </val>
</def>

<def>
  <name> SID </name>
  <val> 0.001 * SIDmEq </val>
</def>

<def>
  <name> CO3Eq </name>
  <val> 2.0 * CO3-- </val>
</def>

<def>
  <name> HEnd </name>
  <val> A- + OH- + HCO3- + CO3Eq - SID </val>
</def>

</implicitmath>

Additional Calculations

<def>
  <name> HA </name>
  <val> ATot - A- </val>
</def>

<def>
  <name> Sc </name>
  <val> 3.0e-5 </val>
</def>

<def>
  <name> CO2 </name>
  <val> Sc * pCO2 </val>
</def>

<def>
  <name> K'c </name>
  <val> 3.1e-3 </val>
</def>

<def>
  <name> H2CO3 </name>
  <val> K'c * CO2 </val>
</def>

Scale Up

<def>
  <name> HIon </name>
  <val> 1e9 * H </val>
</def>

<def>
  <name> pH </name>
  <val> LOG10 ( 1.0 / H ) </val>
</def>

<def>
  <name> OHIon </name>
  <val> 1e9 * OH- </val>
</def>

<def>
  <name> A-mEq </name>
  <val> 1000.0 * A- </val>
</def>

<def>
  <name> HAmEq </name>
  <val> 1000.0 * HA </val>
</def>

<def>
  <name> CO2mMol </name>
  <val> 1000.0 * CO2 </val>
</def>

<def>
  <name> CO2Gas </name>
  <val> 22.4 * CO2mMol </val>
</def>

<def>
  <name> H2CO3uMol </name>
  <val> 1.0e6 * H2CO3 </val>
</def>

<def>
  <name> HCO3-mEq </name>
  <val> 1000.0 * HCO3- </val>
</def>

<def>
  <name> CO3--uMol </name>
  <val> 1e6 * CO3-- </val>
</def>

<def>
  <name> CO3--uEq </name>
  <val>  2.0 * CO3--uMol</val>
</def>

</block>

</definitions>
</structure>

<math>
<dervs> Model.Dervs </dervs>
</math>

<display> =========================================
<panel>

<name> Peter Stewart's pH Calculator </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<showvalue>
   <row> 2.0 </row><col> 1 </col>
   <name> pH </name>
   <format><decimal> 2 </decimal></format>
   <label> pH </label>
</showvalue>

<showvalue>
   <row> 3 </row><col> 1 </col>
   <name> HIon </name>
   <format> <integer/> </format>
   <label> [ H+ ] nEq / L </label>
</showvalue>

<showvalue>
   <row> 4 </row><col> 1 </col>
   <name> OHIon </name>
   <format> <integer/> </format>
   <label> [ OH- ] nEq / L </label>
</showvalue>

<repeatlist>
  <name> ATotmEq </name>
  <repeat>
    <reps> 50 </reps>
    <stepsize> 1 </stepsize>
  </repeat>
  <repeat>
    <reps> 5 </reps>
    <stepsize> 10 </stepsize>
  </repeat>
  <repeat>
    <reps> 9 </reps>
    <stepsize> 100 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 6 </row><col> 1 </col>
  <name> ATotmEq </name>
  <listname> ATotmEq </listname>
  <label> Total Weak Acid (mEq/L) </label>
</slidebar>

<showvalue>
   <row> 7.2 </row><col> 1 </col>
   <name> A-mEq </name>
   <format> <integer/> </format>
   <label> [ A- ] mEq / L </label>
</showvalue>

<showvalue>
   <row> 8.2 </row><col> 1 </col>
   <name> HAmEq </name>
   <format> <integer/> </format>
   <label> [ HA ] mMol / L </label>
</showvalue>

<repeatlist>
  <name> SIDmEq </name>
  <repeat>
    <reps> 100 </reps>
    <stepsize> 1 </stepsize>
  </repeat>
  <repeat>
    <reps> 9 </reps>
    <stepsize> 100 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 10.0 </row><col> 1 </col>
  <name> SIDmEq </name>
  <listname> SIDmEq </listname>
  <label> Strong Ion Difference (mEq/L) </label>
</slidebar>

<repeatlist>
  <name> pCO2 </name>
  <repeat>
    <reps> 100 </reps>
    <stepsize> 1 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 12 </row><col> 1 </col>
  <name> pCO2 </name>
  <listname> pCO2 </listname>
  <label> pCO2 ( mmHg ) </label>
</slidebar>

<showvalue>
   <row> 13.2 </row><col> 3 </col>
   <name> CO2mMol </name>
   <format><decimal> 1 </decimal></format>
   <label> Dissolved CO2 ( mMol / L ) </label>
</showvalue>

<showvalue>
   <row> 13.2 </row><col> 33 </col>
   <name> CO2Gas </name>
   <format> <integer/> </format>
   <label> ( mL / L ) </label>
</showvalue>

<showvalue>
   <row> 15 </row><col> 1 </col>
   <name> H2CO3uMol </name>
   <format><decimal> 1 </decimal></format>
   <label> [ H2CO3 ] uMol / L </label>
</showvalue>

<showvalue>
   <row> 16 </row><col> 1 </col>
   <name> HCO3-mEq </name>
   <format> <integer/> </format>
   <label> [ HCO3- ] mEq / L </label>
</showvalue>

<showvalue>
   <row> 17 </row><col> 1 </col>
   <name> CO3--uMol </name>
   <format> <integer/> </format>
   <label> [ CO3- - ] uMol / L </label>
</showvalue>

<showvalue>
   <row> 17 </row><col> 22 </col>
   <name> CO3--uEq </name>
   <format> <integer/> </format>
   <label> uEq / L </label>
</showvalue>

Right-hand Side ===================================

<label>
  <row> 0.5 </row><col> 41 </col>
  <text> Math </text>
</label>

<infobutton>
  <row> 2.5 </row><col> 41 </col>
  <label> General </label>
  <line> The determinants of pH are </line>
  <line>  </line>
  <line> * Water Dissociation </line>
  <line> * Weak Acid Dissociation and Conservation </line>
  <line> * Bicarbonate and Carbonate Formation </line>
  <line> * Electrical Neutrality </line>
  <line>  </line>
  <line> Values represent plasma at 37C. Units are </line>
  <line> Eq / L. </line>
</infobutton>

<infobutton>
  <row> 3.9 </row><col> 41 </col>
  <label> Water Dissociation </label>
  <line> Water Dissociation </line>
  <line>  </line>
  <line> [ H+ ] * [ OH- ] = K'w </line>
  <line> K'w = 4.4e-14 </line>
</infobutton>

<infobutton>
  <row> 5.3 </row><col> 41 </col>
  <label> Weak Acids </label>
  <line> Weak Acid Dissociation </line>
  <line>  </line>
  <line> [ H+ ] * [ A- ] = Ka * [ HA ] </line>
  <line> Ka = 2.0e-7 </line>
  <line>  </line>
  <line> Weak Acid Conservation </line>
  <line>  </line>
  <line> [ HA ] + [ A- ] = [ ATot ] </line>
</infobutton>

<infobutton>
  <row> 6.7 </row><col> 41 </col>
  <label> [ HCO3- ] and [ CO3- - ] </label>
  <line> Bicarbonate Formation </line>
  <line>  </line>
  <line> [ H+ ] * [ HCO3- ] = Kc * pCO2 </line>
  <line> Kc = 2.58e-11 </line>
  <line>  </line>
  <line> Carbonate Formation </line>
  <line>  </line>
  <line> [ H+ ] * [ CO3- - ] = K3 * [ HCO3- ] </line>
  <line> K3 = 6.0e-11 </line>
</infobutton>

<infobutton>
  <row> 8.1 </row><col> 41 </col>
  <label> Electrical Neutrality </label>
  <line> Electrical Neutrality </line>
  <line>  </line>
  <line> + [ SID ] </line>
  <line> + [ H+ ] </line>
  <line> - [ HCO3- ] </line>
  <line> - [ A- ] </line>
  <line> - [ CO3- - ] </line>
  <line> - [ OH- ] </line>
  <line>  </line>
  <line> = 0 </line>
</infobutton>

</panel>
</display>

</model>

End
Pendulum

Created : 12-Aug-96
Last Modified : 01-Dec-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

A pendulum accellerates downward gaining
momentum.  After it passes the bottom, it
loses momentum and slows, eventually changing
direstion.

Of interest: A pendulum oscillates at a regular
interval.

Since the trajectory of a pendulum is restricted
to an arc on a circle, use of polar coordinates
is appropriate.

<?xml version = '1.0' ?>

<model>

<title><basic> Pendulum </basic></title>

<structure>
<name> Model </name>

<variables> ========================================

<parm>
  <name> InitAngle </name>
  <val> -30 </val>
</parm>

Acceleration for this pendulum in the y direction
is gravity.  The length of the pendulum is a factor.

<var>
  <name> Length </name>
 
</var>
<!-- Initial length added to substitute  Length --> 

<parm>
  <name> InitialLength </name>
  <val> 1.0 </val>
</parm>


<parm>
  <name> Growth </name>
  <val> 0.1 </val>
</parm>


<parm>
  <name> Gravity </name>
  <val> 9.8 </val>
</parm>

<var>
  <name> XPos </name>
</var>

<var>
  <name> YPos </name>
</var>

<var>
  <name> AngleAcc </name>
</var>

<var>
  <name> AngleDeg </name>
</var>

<var>
  <name> VelocityDeg </name>
</var>

</variables>

<equations> ========================================

Angular velocity is the integral over time of
acceleration.

<diffeq>
  <name> Velocity </name>
  <integralname> Velocity </integralname>
  <initialval> 0.0 </initialval>
  <dervname> dVelocity/dt </dervname>
  <errorlim> 1E-3 </errorlim>
</diffeq>

The angular position of the pendulum is the integral
over time of its angular velocity.

<diffeq>
  <name> Angle </name>
  <integralname> Angle </integralname>
  <initialval> 0.0 </initialval>
  <dervname> dAngle/dt </dervname>
  <errorlim> 1E-5 </errorlim>
</diffeq>

</equations>

<definitions> =======================================

<block><name> Parms </name> =======================

The initial angle can be adusted.

<def>
  <name> Angle </name>
  <val> DEGTORAD ( InitAngle ) </val>
</def>

</block>

<block><name> Dervs </name> =========================

Calculate the X and Y coordinates.

<def> <name> Length  </name> 
<val> InitialLength + Growth * System.X </val>
</def> 

<def>
  <name> XPos </name>
  <val> Length * COS ( Angle ) </val>
</def>

<def>
  <name> YPos </name>
  <val> - Length * SIN ( Angle ) </val>
</def>

Angular acceleration.  It's a derivative.

<def>
  <name> dVelocity/dt </name>
  <val> ( Gravity / Length ) * COS Angle </val>
</def>

<def>
  <name> dAngle/dt </name>
  <val> Velocity </val>
</def>

Acceleration and velocity in degrees for display.

<def>
  <name> AngleDeg </name>
  <val> RADTODEG ( Angle ) </val>
</def>

<def>
  <name> VelocityDeg </name>
  <val> RADTODEG ( Velocity ) </val>
</def>

</block>

</definitions>
</structure>

<math>

  <parms> Model.Parms </parms>
  <dervs> Model.Dervs </dervs>

</math>

<control> ========================================

<gofor>
  <solutionint> 2.0 </solutionint>
  <displayint> 0.2 </displayint>
  <storageint> 0.1 </storageint>
</gofor>

</control>

<display> =========================================
<panel>

<name> Pendulum </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

Initial Angle

<groupbox>
   <row> 2 </row><col> 1 </col>
   <high> 3.5 </high><wide> 28 </wide>
   <title> Initial Angle </title>

<repeatlist>
  <name> InitAngle </name>
  <firstval> -90 </firstval>
  <repeat>
    <reps> 36 </reps>
    <stepsize> 10 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 1.5 </row><col> 1 </col>
  <wide> 8 </wide>
  <name> InitAngle </name>
  <listname> InitAngle </listname>
  <label> Degrees </label>
</slidebar>

</groupbox>

Length

<groupbox>
   <row> 6.0 </row><col> 1 </col>
   <high> 3.5 </high><wide> 28 </wide>
   <title> Length </title>

<repeatlist>
  <name> Length </name>
  <firstval> 0.5 </firstval>
  <repeat>
    <reps> 9 </reps>
    <stepsize> 0.5 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 1.5 </row><col> 1 </col>
  <name> Length </name>
  <listname> Length </listname>
  <label> Length </label>
</slidebar>

</groupbox>

<showvalue>
   <row> 10 </row><col> 1 </col>
   <name> AngleDeg </name>
   <format> <integer/> </format>
   <label> Angle (Deg) </label>
</showvalue>

<infobutton>
  <row> 11.5 </row><col> 1 </col>
  <line> Multiply degrees by PI / 180 </line>
  <line> to convert to radians. </line>
</infobutton>

<showvalue>
   <row> 11.5 </row><col> 4 </col>
   <name> Angle </name>
   <format><decimal> 2 </decimal></format>
   <label> Angle (Rad) </label>
</showvalue>

<showvalue>
   <row> 13 </row><col> 1 </col>
   <name> VelocityDeg </name>
   <format> <integer/> </format>
   <label> VelocityDeg </label>
</showvalue>

<showvalue>
   <row> 14 </row><col> 1 </col>
   <name> Velocity </name>
   <format><decimal> 2 </decimal></format>
   <label> Velocity </label>
</showvalue>

<showvalue>
   <row> 15.5 </row><col> 1 </col>
   <name> System.X </name>
   <format><decimal> 1 </decimal></format>
   <label> Time </label>
</showvalue>

Right-hand Side ===================================

<symbol>
  <name> Box </name>
  <style> BOX </style>
  <color> RED </color>
</symbol>

<showgraph>
  <row> 2.0 </row><col> 32 </col>
  <high> 14 </high><wide> 34 </wide>
  <leftmargin> 4 </leftmargin>

  <xaxis>
    <name> XPos </name>
    <label> X </label>
    <scale><min> -1.3 </min><max> 1.3 </max><inc> 1.0 </inc></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> YPos </name>
      <label> Y </label>
      <linetype> NULL </linetype>
      <symbolname> Box </symbolname>
    </yvar>
    <scale><min> -1.3 </min><max> 1.3 </max><inc> 1.0 </inc></scale>
  </yaxis>
</showgraph>

<label>
  <row> 18 </row><col> 32 </col>
  <text> Zero degrees in this model is </text>
</label>

<label>
  <row> 19 </row><col> 32 </col>
  <text> horizontal and to the right. </text>
</label>

</panel>
</display>

</model>

End
Predator-Prey

Created : 24-Aug-96
Last Modified : 02-Dec-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

The life of predators (wolves) depends
on their success in consuming prey (moose).

The independent variable is time in years.

<?xml version = '1.0' ?>

<model>

<title><basic> Predator-Prey </basic></title>

<structure><name> Model </name> =============

<variables>

<var><name> MooseBirths </name></var>
<var><name> MooseDeaths </name></var>
<var><name> WolfBirths </name></var>
<var><name> WolfDeaths </name></var>

<parm>
  <name> InitialMooseCount </name>
  <val> 1500 </val>
</parm>

<parm>
  <name> InitialWolfCount </name>
  <val> 50 </val>
</parm>

These next parameters define the moose and
wolf interactions.

<parm>
  <name> MooseBirthRate </name>
  <val> 0.1 </val>
</parm>

<var>
  <name> MooseDeathRate </name>
</var>

<parm>
  <name> WolfHuntingSkills </name>
  <val> 0.002 </val>
</parm>

<parm>
  <name> MooseFoodValue </name>
  <val> 0.1 </val>
</parm>

<var><name> WolfBirthRate </name></var>

<parm>
  <name> WolfDeathRate </name>
  <val> 0.2 </val>
</parm>

Use these variables to track maximum and
minimum animal count.

<var>
  <name> FirstTime </name>
  <val> TRUE </val>
</var>

<var>
  <name> MinimumMoose </name>
  <val> UNDEFINED </val>
</var>

<var>
  <name> MaximumMoose </name>
  <val> UNDEFINED </val>
</var>

<var>
  <name> MinimumWolves </name>
  <val> UNDEFINED </val>
</var>

<var>
  <name> MaximumWolves </name>
  <val> UNDEFINED </val>
</var>

</variables>

<equations> ---------------------------------

The moose and wolf head counts are equal
to the integral over time of their net
rate of change.

<diffeq>
  <name> MooseCount </name>
  <integralname> MooseCount </integralname>
  <initialval> 1500.0 </initialval>
  <dervname> MooseCountChange </dervname>
  <errorlim> 1E-2 </errorlim>
</diffeq>

<diffeq>
  <name> WolfCount </name>
  <integralname> WolfCount </integralname>
  <initialval> 50.0 </initialval>
  <dervname> WolfCountChange </dervname>
  <errorlim> 1E-3 </errorlim>
</diffeq>

</equations>

<definitions> -----------------------------------

<block> <name> Parms </name>

<def>
  <name> MooseCount </name>
  <val> InitialMooseCount </val>
</def>

<def>
  <name> WolfCount </name>
  <val> InitialWolfCount </val>
</def>

</block>

<block> <name> Dervs </name>

Moose births is equal to the product of the birth
rate and the number of living moose.

<def>
  <name> MooseBirths </name>
  <val> MooseBirthRate * MooseCount </val>
</def>

Moose death rate is the product of wolf hunting
skills and the size of the wolf population.

<def>
  <name> MooseDeathRate </name>
  <val> WolfHuntingSkills * WolfCount </val>
</def>

Moose deaths is equal to the product of the death
rate and the number of living moose.

<def>
  <name> MooseDeaths </name>
  <val> MooseDeathRate * MooseCount </val>
</def>

Wolf birth rate is the product of the nutritional
value of moose, wolf hunting skills and the size
of the moose population.

<def>
  <name> WolfBirthRate </name>
  <val> MooseFoodValue * WolfHuntingSkills * MooseCount </val>
</def>

Wolf births is equal to the product of the birth
rate and the number of living wolves.

<def>
  <name> WolfBirths </name>
  <val> WolfBirthRate * WolfCount </val>
</def>

Wolf deaths is equal to the product of the death
rate and the number of living wolves.

<def>
  <name> WolfDeaths </name>
  <val> WolfDeathRate * WolfCount </val>
</def>

<def>
  <name> MooseCountChange </name>
  <val> MooseBirths - MooseDeaths </val>
</def>

<def>
  <name> WolfCountChange </name>
  <val> WolfBirths - WolfDeaths </val>
</def>

<if><test> FirstTime </test>

<true>

  <def>
    <name> FirstTime </name>
    <val> FALSE </val>
  </def>

  <def>
    <name> MinimumWolves </name>
    <val> WolfCount </val>
  </def>

  <def>
    <name> MaximumWolves </name>
    <val> WolfCount </val>
  </def>

  <def>
    <name> MinimumMoose </name>
    <val> MooseCount </val>
  </def>

  <def>
    <name> MaximumMoose </name>
    <val> MooseCount </val>
  </def>

</true>

<false>
  <def>
    <name> MinimumMoose </name>
    <val> MinimumMoose MIN MooseCount </val>
  </def>

  <def>
    <name> MaximumMoose </name>
    <val> MaximumMoose MAX MooseCount </val>
  </def>

  <def>
    <name> MinimumWolves </name>
    <val> MinimumWolves MIN WolfCount </val>
  </def>

  <def>
    <name> MaximumWolves </name>
    <val> MaximumWolves MAX WolfCount </val>
  </def>

  </false>
</if>
</block>

</definitions>
</structure>

<math> --------------------------------------

<parms> Model.Parms </parms>
<dervs> Model.Dervs </dervs>

</math>

<control> ---------------------------------------

<gofor>
  <solutionint> 100 </solutionint>
  <displayint> 1 </displayint>
  <storageint> 1 </storageint>
</gofor>

</control>

<display> ---------------------------------------

<panel><name> Predator-Prey </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<showvalue>
  <row> 2.5 </row><col> 1 </col>
  <name> WolfCount </name>
  <format><integer/></format>
</showvalue>

<showvalue>
  <row> 3.5 </row><col> 4 </col>
  <name> WolfBirths </name>
  <format><integer/></format>
</showvalue>

<showvalue>
  <row> 4.5 </row><col> 4 </col>
  <name> WolfDeaths </name>
  <format><integer/></format>
</showvalue>

<showvalue>
  <row> 6.5 </row><col> 1 </col>
  <name> MooseCount </name>
  <format><integer/></format>
</showvalue>

<showvalue>
  <row> 7.5 </row><col> 4 </col>
  <name> MooseBirths </name>
  <format><integer/></format>
</showvalue>

<showvalue>
  <row> 8.5 </row><col> 4 </col>
  <name> MooseDeaths </name>
  <format><integer/></format>
</showvalue>

<editbox>
  <row> 10.5 </row><col> 1 </col>
  <name> InitialMooseCount </name>
  <label> Initial Moose </label>
</editbox>

<editbox>
  <row> 12.1 </row><col> 1 </col>
  <name> InitialWolfCount </name>
  <label> Initial Wolves </label>
</editbox>

Right-hand Side -------------------------

<showvalue>
  <row> 0.5 </row><col> 32 </col>
  <name> System.X </name>
  <format><integer/></format>
  <label> Time </label>
</showvalue>

<infobutton>
  <row> 0.5 </row><col> 49 </col>
  <label> Reference </label>
  <line> This model is the Lotka-Voltera </line>
  <line> model.  Coefficients are from </line>
  <line>  </line>
  <line> Spain, J. </line>
  <line> 1982 </line>
  <line> BASIC Microcomputer Models in Biology </line>
  <line> Addison-Wesley  Reading, MA </line>
  <line> pp. 102-103 </line>
</infobutton>

<symbol>
  <name> dot </name>
  <style> CIRCLE </style>
  <color> RED </color>
  <size> 3 </size>
</symbol>

<showgraph>
  <row> 3.0 </row><col> 32 </col>
  <high> 14 </high><wide> 30 </wide>
  <leftmargin> 5 </leftmargin>

  <xaxis>
    <name> WolfCount </name>
    <label> Wolves </label>
    <scale><min> 20 </min><max> 90 </max><inc> 10 </inc></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> MooseCount </name>
      <label> Moose </label>
      <linecolor> RED </linecolor>
      <symbolname> dot </symbolname>
    </yvar>
    <scale><min> 600 </min><max> 1600 </max><inc> 100 </inc></scale>
  </yaxis>
</showgraph>

<showvalue>
  <row> 18.5 </row><col> 32 </col>
  <name> MinimumWolves </name>
  <format><integer/></format>
  <label> Wolves Min </label>
</showvalue>

<showvalue>
  <row> 18.5 </row><col> 51 </col>
  <name> MaximumWolves </name>
  <format><integer/></format>
  <label> Max </label>
</showvalue>

<showvalue>
  <row> 19.5 </row><col> 32 </col>
  <name> MinimumMoose </name>
  <format><integer/></format>
  <label> Moose Min </label>
</showvalue>

<showvalue>
  <row> 19.5 </row><col> 51 </col>
  <name> MaximumMoose </name>
  <format><integer/></format>
  <label> Max </label>
</showvalue>

</panel>
</display>

</model>

EndRenal Artery Stenosis

Created : 25-Mar-02
Last Modified : 02-Dec-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

<?xml version = '1.0' ?>

<model>

<title><basic> Renal Artery Stenosis </basic></title>

<structure>
<name> Model </name>

<variables> ========================================

Arterial Pressure

<var>
  <name> ArterialPressure </name>
</var>

<parm>
  <name> NormalArterialPressure </name>
  <val> 100.0 </val>
</parm>

<var>
  <name> ECFV-AP </name>
</var>

<var>
  <name> PRA-AP </name>
</var>

ECFV

<parm>
  <name> Salt&H2OIntake </name>
  <val> 1.0 </val>
</parm>

<var>
  <name> UrineOutput </name>
</var>

Kidney

<var>
  <name> RenalArteryPressure </name>
</var>

<parm>
  <name> RenalArteryPressureDrop </name>
  <val> 0.0 </val>
</parm>

Renin

<var>
  <name> PRA </name>
</var>

<var>
  <name> ReninSecretion </name>
</var>

<parm>
  <name> ReninSecBlock </name>
  <val> 1.0 </val>
</parm>

<var>
  <name> ReninDegradation </name>
</var>

<parm>
  <name> ReninClearanceK </name>
  <val> 0.07 </val>
</parm>

</variables>

<equations> ========================================

<diffeq>
  <name> ECFV </name>
  <integralname> ECFV </integralname>
  <initialval> 15000 </initialval>
  <dervname> ECFVChange </dervname>
  <errorlim> 150 </errorlim>
</diffeq>

<diffeq>
  <name> Renin </name>
  <integralname> Renin </integralname>
  <initialval> 30000 </initialval>
  <dervname> ReninChange </dervname>
  <errorlim> 300 </errorlim>
</diffeq>

{ /equations }

</equations>

<functions> ========================================

<curve>
  <name> ECFV-AP </name>
  <point>
    <x> 8000 </x><y> 0 </y><slope> 0 </slope>
  </point>
  <point>
    <x> 15000 </x><y> 1.0 </y><slope> 0.0004 </slope>
  </point>
  <point>
    <x> 20000 </x><y> 2.0 </y><slope> 0 </slope>
  </point>
</curve>

<curve>
  <name> PRA-AP </name>
  <point>
    <x> 0 </x><y> 0.8 </y><slope> 0 </slope>
  </point>
  <point>
    <x> 2.0 </x><y> 1.0 </y><slope> 0.2 </slope>
  </point>
  <point>
    <x> 10.0 </x><y> 2.0 </y><slope> 0 </slope>
  </point>
</curve>

<curve>
  <name> RAP-ReninSecretion </name>
  <point>
    <x> 40 </x><y> 10000 </y><slope> 0 </slope>
  </point>
  <point>
    <x> 100 </x><y> 2100 </y><slope> -50.0 </slope>
  </point>
  <point>
    <x> 200 </x><y> 0 </y><slope> 0 </slope>
  </point>
</curve>

<curve>
  <name> RAP-UrineOutput </name>
  <point>
    <x> 60 </x><y> 0.0 </y><slope> 0 </slope>
  </point>
  <point>
    <x> 100 </x><y> 1.0 </y><slope> 0.04 </slope>
  </point>
  <point>
    <x> 200 </x><y> 4.0 </y><slope> 0 </slope>
  </point>
</curve>

</functions>

<definitions> =======================================

<block><name> Dervs </name> =========================

<def>
  <name> PRA </name>
  <val> Renin / ECFV </val>
</def>

<def>
  <name> ECFV-AP </name>
  <val> ECFV-AP [ ECFV ] </val>
</def>

<def>
  <name> PRA-AP </name>
  <val> PRA-AP [ PRA ] </val>
</def>

<def>
  <name> ArterialPressure </name>
  <val>
      NormalArterialPressure
    * ECFV-AP
    * PRA-AP
  </val>
</def>

<def>
  <name> RenalArteryPressure </name>
  <val>
      ArterialPressure
    - RenalArteryPressureDrop
  </val>
</def>

<def>
  <name> UrineOutput </name>
  <val>
      RAP-UrineOutput
    [ RenalArteryPressure ]
  </val>
</def>

<def>
  <name> ECFVChange </name>
  <val>
      Salt&H2OIntake
    - UrineOutput
  </val>
</def>

<def>
  <name> ReninSecretion </name>
  <val>
      ReninSecBlock
    * RAP-ReninSecretion
    [ RenalArteryPressure ]
  </val>
</def>

<def>
  <name> ReninDegradation </name>
  <val> Renin * ReninClearanceK </val>
</def>

<def>
  <name> ReninChange </name>
  <val> ReninSecretion - ReninDegradation </val>
</def>

</block>

</definitions>
</structure>

<math> =============================================

  <dervs> Model.Dervs </dervs>

</math>

<control> ========================================

<gofor>
  <solutionint> 10 </solutionint>
  <displayint> 1 </displayint>
  <menuitem> 10 Min </menuitem>
</gofor>

<gofor>
  <solutionint> 60 </solutionint>
  <displayint> 5 </displayint>
  <menuitem> 1 Hour </menuitem>
</gofor>

<gofor>
  <solutionint> 1440 </solutionint>
  <displayint> 120 </displayint>
  <menuitem> 1 Day </menuitem>
</gofor>

<gofor>
  <solutionint> 10080 </solutionint>
  <displayint> 1440 </displayint>
  <menuitem> 1 Week </menuitem>
</gofor>

<gobar/>

<goto>
  <solutionint> 60 </solutionint>
  <displayint> 5 </displayint>
  <menuitem> Next Hour </menuitem>
</goto>

<goto>
  <solutionint> 1440 </solutionint>
  <displayint> 120 </displayint>
  <menuitem> Next Day </menuitem>
</goto>

<goto>
  <solutionint> 10080 </solutionint>
  <displayint> 1440 </displayint>
  <menuitem> Next Week </menuitem>
</goto>

</control>

<display> =========================================

<common>

<tree>

<branch>
  <name> Panels </name>
  <label> Panels </label>
  <parent> MAINMENU </parent>
</branch>

<leaf>
  <name> General </name>
  <label> General </label>
  <parent> Panels </parent>
</leaf>

<leaf>
  <name> ECFV </name>
  <label> ECFV </label>
  <parent> Panels </parent>
</leaf>

<leaf>
  <name> Renin </name>
  <label> Renin </label>
  <parent> Panels </parent>
</leaf>

</tree>

</common>

<panel><name> General </name> ===========================

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<showclock>
   <row> 0.5 </row><col> 36 </col>
   <name> System.X </name>
   <ampm/>
   <days/>
</showclock>

<showvalue>
   <row> 2.0 </row><col> 1 </col>
   <name> ArterialPressure </name>
   <format> <integer/> </format>
   <label> ArterialPressure </label>
</showvalue>

<showgraph>
  <row> 3.0 </row><col> 1 </col>
  <high> 7 </high><wide> 30 </wide>
  <leftmargin> 4 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 2 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> ArterialPressure </name>
      <label> AP </label>
    </yvar>
    <scale><min> 80 </min><max> 120 </max><inc> 20 </inc></scale>
  </yaxis>
</showgraph>

<editbox>
   <row> 10.4 </row><col> 1 </col>
   <name> RenalArteryPressureDrop </name>
   <label> RenalArteryPressureDrop </label>
</editbox>

<showvalue>
   <row> 11.8 </row><col> 1 </col>
   <name> RenalArteryPressure </name>
   <format> <integer/> </format>
   <label> RenalArteryPressure </label>
</showvalue>

<showvalue>
   <row> 13.2 </row><col> 1 </col>
   <name> ECFV-AP </name>
   <format><decimal> 1 </decimal></format>
   <label> ECFV-AP </label>
</showvalue>

<showvalue>
   <row> 14.2 </row><col> 1 </col>
   <name> PRA-AP </name>
   <format><decimal> 1 </decimal></format>
   <label> PRA-AP </label>
</showvalue>

</panel>

<panel><name> ECFV </name> ===========================

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<showclock>
   <row> 0.5 </row><col> 36 </col>
   <name> System.X </name>
   <ampm/>
   <days/>
</showclock>

<showvalue>
   <row> 2.0 </row><col> 1 </col>
   <name> ECFV </name>
   <format> <integer/> </format>
   <label> ECFV </label>
</showvalue>

<showgraph>
  <row> 3.0 </row><col> 1 </col>
  <high> 7 </high><wide> 30 </wide>
  <leftmargin> 7 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 60 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> ECFV </name>
      <label> ECFV </label>
    </yvar>
    <scale><min> 14000 </min><max> 16000 </max><inc> 1000 </inc></scale>
  </yaxis>
</showgraph>

<showvalue>
   <row> 10.4 </row><col> 1 </col>
   <name> ECFVChange </name>
   <format> <integer/> </format>
   <label> ECFVChange </label>
</showvalue>

<editbox>
   <row> 11.8 </row><col> 3 </col>
   <name> Salt&H2OIntake </name>
   <label> Salt&H2OIntake </label>
</editbox>

<showvalue>
   <row> 13.2 </row><col> 3 </col>
   <name> UrineOutput </name>
   <format><decimal> 1 </decimal></format>
   <label> UrineOutput </label>
</showvalue>

<showcurve>
  <row> 2.0 </row><col> 32 </col>
  <high> 6 </high><wide> 24 </wide>
  <leftmargin> 6 </leftmargin>

  <xaxis>
    <name> RenalArteryPressure </name>
    <label> RAP </label>
    <scale><min> 0 </min><max> 200 </max><inc> 100 </inc></scale>
  </xaxis>

  <yaxis>
    <label> UO </label>
    <scale><min> 0 </min><max> 4 </max><inc> 1 </inc></scale>
  </yaxis>

  <curvename> RAP-UrineOutput </curvename>
</showcurve>

<showcurve>
  <row> 9.0 </row><col> 32 </col>
  <high> 6 </high><wide> 24 </wide>
  <leftmargin> 6 </leftmargin>

  <xaxis>
    <name> ECFV </name>
    <label> ECFV </label>
    <scale><min> 0 </min><max> 20000 </max><inc> 1000 </inc></scale>
  </xaxis>

  <yaxis>
    <label> AP Eff </label>
    <scale><min> 0 </min><max> 2 </max><inc> 1 </inc></scale>
  </yaxis>

  <curvename> ECFV-AP </curvename>
</showcurve>

</panel>

<panel><name> Renin </name> ===========================

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<showclock>
   <row> 0.5 </row><col> 36 </col>
   <name> System.X </name>
   <ampm/>
   <days/>
</showclock>

<showvalue>
   <row> 2.0 </row><col> 1 </col>
   <name> PRA </name>
   <format><decimal> 1 </decimal></format>
   <label> PRA </label>
</showvalue>

<showgraph>
  <row> 3.0 </row><col> 1 </col>
  <high> 7 </high><wide> 30 </wide>
  <leftmargin> 4 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 60 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> PRA </name>
      <label> PRA </label>
    </yvar>
    <scale><min> 1 </min><max> 3 </max><inc> 1 </inc>
    </scale>
  </yaxis>
</showgraph>

<showvalue>
   <row> 11.4 </row><col> 1 </col>
   <name> Renin </name>
   <format> <integer/> </format>
   <label> Renin </label>
</showvalue>

<showvalue>
   <row> 12.4 </row><col> 3 </col>
   <name> ReninChange </name>
   <format> <integer/> </format>
   <label> Renin Change </label>
</showvalue>

<showvalue>
   <row> 13.8 </row><col> 1 </col>
   <name> ReninSecretion </name>
   <format> <integer/> </format>
   <label> Renin Secretion </label>
</showvalue>

<editbox>
   <row> 15.2 </row><col> 3 </col>
   <name> ReninSecBlock </name>
   <label> Renin Sec Block </label>
</editbox>

<showvalue>
   <row> 16.6 </row><col> 1 </col>
   <name> ReninDegradation </name>
   <format> <integer/> </format>
   <label> Renin Degradation </label>
</showvalue>

<editbox>
   <row> 18.0 </row><col> 3 </col>
   <name> ReninClearanceK </name>
   <label> Renin Clearance K </label>
</editbox>

<showcurve>
  <row> 2.0 </row><col> 32 </col>
  <high> 6 </high><wide> 24 </wide>
  <leftmargin> 6 </leftmargin>

  <xaxis>
    <name> RenalArteryPressure </name>
    <label> RAP </label>
    <scale><min> 0 </min><max> 200 </max><inc> 100 </inc></scale>
  </xaxis>

  <yaxis>
    <label> Sec </label>
    <scale><min> 0 </min><max> 10000 </max><inc> 1000 </inc></scale>
  </yaxis>

  <curvename> RAP-ReninSecretion </curvename>
</showcurve>

<showcurve>
  <row> 9.0 </row><col> 32 </col>
  <high> 6 </high><wide> 24 </wide>
  <leftmargin> 6 </leftmargin>

  <xaxis>
    <name> PRA </name>
    <label> PRA </label>
    <scale><min> 0 </min><max> 10 </max><inc> 1 </inc></scale>
  </xaxis>

  <yaxis>
    <label> AP Eff </label>
    <scale><min> 0 </min><max> 2 </max><inc> 1 </inc></scale>
  </yaxis>

  <curvename> PRA-AP </curvename>
</showcurve>

</panel>
</display>

</model>

End
Simple DE

Created : 30-Mar-09
Last Modified : 30-Mar-09
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center

<?xml version = '1.0' ?>

<model>

<title><basic> Simple DE </basic></title>

<structure> <name> Model </name>

<variables> ------------------------------------

<parm><name> K </name><val> 0.1 </val></parm>

</variables>

<equations> ------------------------------------

<diffeq>
  <name> Y </name>
  <integralname> Y </integralname>
  <initialval> 1.0 </initialval>
  <dervname> dY/dX </dervname>
</diffeq>

</equations>

<definitions> ----------------------------------

<block><name> Dervs </name>

<def>
  <name> dY/dX </name>
  <val> - K * Y </val>
</def>

</block>
</definitions>
</structure>

<math> --------------------------------------

<dervs> Model.Dervs </dervs>

</math>

<control> ---------------------------------------

<gofor>
  <solutionint> 10 </solutionint>
  <displayint> 1 </displayint>
  <menuitem> 10 </menuitem>
</gofor>

</control>

<display> ---------------------------------------

<panel> <name> Simple DE </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<showvalue>
  <row> 2 </row><col> 1 </col>
  <name> Y </name>
  <format><decimal> 5 </decimal></format>
  <label> Y </label>
</showvalue>

<showvalue>
  <row> 3 </row><col> 1 </col>
  <name> dY/dX </name>
  <format><decimal> 5 </decimal></format>
  <label> dY/dX </label>
</showvalue>

<showgraph>
  <row> 2.5 </row><col> 28 </col>
  <high> 7 </high><wide> 32 </wide>
  <leftmargin> 8 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> X </label>
    <scale><min> 0 </min><max> 10 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> Y </name>
      <label> Y </label>
    </yvar>
			   
    <scale><min> 0 </min><max> 1 </max></scale>
  </yaxis>

</showgraph>

</panel>
</display>

</model>

EndSkeletal Muscle Atrophy

Created : 23-Feb-2008
Last Modified : 02-Dec-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

<?xml version = '1.0' ?>

<model>

<title><basic> Skeletal Muscle Atrophy </basic></title>

<structure>
<name> Model </name>

<variables> ======================================

Muscle activity

   0.0 = Max Unload
   0.5 = Unload
   1.0 = Normal
   1.5 = Load
   2.0 = Max Load

<parm>
  <name> MuscleActivity </name>
  <val> 1.0 </val>
</parm>

<var>
  <name> SynthesisRate </name>
</var>

<parm>
  <name> NormalSynthesisRate </name>
  <val> 0.0006667 </val> %/Min
</parm>

<var>
  <name> SynthesisRateMultiplier </name>
</var>

<var>
  <name> DegradationRate </name>
</var>

<parm>
  <name> NormalDegradationK </name>
  <val> 0.000006667 </val> /Min
</parm>

<var>
  <name> DegradationRateMultiplier </name>
</var>

<var>
  <name> K12 </name>
</var>

<parm>
  <name> K12Basic </name>
  <val> 6.9e-06 </val> /Min
</parm>

<var>
  <name> K12Multiplier </name>
</var>

<var>
  <name> K21 </name>
</var>

<parm>
  <name> K21Basic </name>
  <val> 10.35e-06 </val> /Min
</parm>

<var>
  <name> K21Multiplier </name>
</var>

</variables>

<equations> =====================================

Muscle mass is defined in terms of % of normal.

<diffeq>
  <name> MuscleMass </name>
  <integralname> MuscleMass </integralname>
  <initialval> 100.0 </initialval>
  <dervname> MuscleMassChange </dervname>
  <errorlim> 1.0 </errorlim>
</diffeq>

<diffeq>
  <name> TypeI(%) </name>
  <integralname> TypeI(%) </integralname>
  <initialval> 60.0 </initialval>
  <dervname> TypeIChange </dervname>
  <errorlim> 1.0 </errorlim>
</diffeq>

<diffeq>
  <name> TypeII(%) </name>
  <integralname> TypeII(%) </integralname>
  <initialval> 40.0 </initialval>
  <dervname> TypeIIChange </dervname>
  <errorlim> 1.0 </errorlim>
</diffeq>

</equations>

<functions> ====================================

<curve>
  <name> SynthesisRateMultiplier </name>
  <point><x> 0 </x><y> 0.5 </y><slope> 0 </slope></point>
  <point><x> 1 </x><y> 1.0 </y><slope> 1.5 </slope></point>
  <point><x> 2 </x><y> 2.5 </y><slope> 0 </slope></point>
</curve>

<curve>
  <name> DegradationRateMultiplier </name>
  <point><x> 0 </x><y> 2.0 </y><slope> 0 </slope></point>
  <point><x> 1 </x><y> 1.0 </y><slope> -1.0 </slope></point>
  <point><x> 2 </x><y> 0.5 </y><slope> 0 </slope></point>
</curve>

<curve>
  <name> K12Multiplier </name>
  <point><x> 0 </x><y> 1.5 </y><slope> 0 </slope></point>
  <point><x> 1 </x><y> 1.0 </y><slope> -1.0 </slope></point>
  <point><x> 2 </x><y> 0.5 </y><slope> 0 </slope></point>
</curve>

</functions>

<definitions> ===================================

<block><name> Dervs </name> =====================

<def>
  <name> SynthesisRateMultiplier </name>
  <val> SynthesisRateMultiplier [ MuscleActivity ] </val>
</def>

<def>
  <name> SynthesisRate </name>
  <val> NormalSynthesisRate * SynthesisRateMultiplier </val>
</def>

<def>
  <name> DegradationRateMultiplier </name>
  <val> DegradationRateMultiplier [ MuscleActivity ] </val>
</def>

<def>
  <name> DegradationRate </name>
  <val> NormalDegradationK * DegradationRateMultiplier * MuscleMass  </val>
</def>

<def>
  <name> MuscleMassChange </name>
  <val> SynthesisRate - DegradationRate </val>
</def>

<def>
  <name> K12Multiplier </name>
  <val> K12Multiplier [ MuscleActivity ] </val>
</def>

<def>
  <name> K12 </name>
  <val> K12Multiplier * K12Basic </val>
</def>

<def>
  <name> K21Multiplier </name>
  <val> 1.0 </val>
</def>

<def>
  <name> K21 </name>
  <val> K21Multiplier * K21Basic </val>
</def>

<def>
  <name> TypeIChange </name>
  <val> ( K21 * TypeII(%) ) - ( K12 * TypeI(%) ) </val>
</def>

<def>
  <name> TypeIIChange </name>
  <val> ( K12 * TypeI(%) ) - ( K21 * TypeII(%) ) </val>
</def>

</block>

</definitions>
</structure>

<math>
  <dervs> Model.Dervs </dervs>
</math>

<control> ========================================

The time base for QHP is minutes, so I'll go with
minutes here.

<gofor>
  <solutionint> 60 </solutionint>
  <displayint> 10 </displayint>
  <menuitem> 1 Hour </menuitem>
</gofor>

<gofor>
  <solutionint> 1440 </solutionint>
  <displayint> 60 </displayint>
  <menuitem> 1 Day </menuitem>
</gofor>

<gofor>
  <solutionint> 43200 </solutionint>
  <displayint> 1440 </displayint>
  <menuitem> 30 Days </menuitem>
</gofor>

</control>

<display> =========================================
<panel>

<name> Skeletal Muscle Atrophy </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<maplist>
  <name> MuscleActivity </name>
  <map><val> 0.0 </val><img> Max Unload </img></map>
  <map><val> 0.5 </val><img> Unload </img></map>
  <map><val> 1.0 </val><img> Normal </img></map>
  <map><val> 1.5 </val><img> Load </img></map>
  <map><val> 2.0 </val><img> Max Load </img></map>
</maplist>

<slidebar>
  <row> 2.4 </row><col> 1 </col>
  <wide> 8.0 </wide>
  <name> MuscleActivity </name>
  <listname> MuscleActivity </listname>
  <label> Activity </label>
</slidebar>

<showvalue>
   <row> 3.8 </row><col> 1 </col>
   <name> MuscleMass </name>
   <format><decimal> 1 </decimal></format>
   <label> Muscle Mass </label>
</showvalue>

<showvalue>
   <row> 4.8 </row><col> 3 </col>
   <name> MuscleMassChange </name>
   <format><decimal> 6 </decimal></format>
   <label> Change </label>
</showvalue>

<showvalue>
   <row> 6.2 </row><col> 1 </col>
   <name> SynthesisRate </name>
   <format><decimal> 6 </decimal></format>
   <label> Synthesis Rate </label>
</showvalue>

<showvalue>
   <row> 7.2 </row><col> 3 </col>
   <name> NormalSynthesisRate </name>
   <format><decimal> 6 </decimal></format>
   <label> Normal </label>
</showvalue>

<showvalue>
   <row> 8.2 </row><col> 3 </col>
   <name> SynthesisRateMultiplier </name>
   <format><decimal> 2 </decimal></format>
   <label> Activity Factor </label>
</showvalue>

<showvalue>
   <row> 9.6 </row><col> 1 </col>
   <name> DegradationRate </name>
   <format><decimal> 6 </decimal></format>
   <label> Degradation Rate </label>
</showvalue>

<showvalue>
   <row> 10.6 </row><col> 3 </col>
   <name> NormalDegradationK  </name>
   <format>
     <decimal> 8 </decimal>
     <fieldwidth> 12 </fieldwidth>
   </format>
   <label> Normal K </label>
</showvalue>

<showvalue>
   <row> 11.6 </row><col> 3 </col>
   <name> DegradationRateMultiplier </name>
   <format><decimal> 2 </decimal></format>
   <label> Activity Factor </label>
</showvalue>

<showvalue>
   <row> 13.0 </row><col> 1 </col>
   <name> TypeI(%) </name>
   <format><decimal> 1 </decimal></format>
   <label> Type I (%) </label>
</showvalue>

<showvalue>
   <row> 14.0 </row><col> 3 </col>
   <name> TypeIChange  </name>
   <format>
     <decimal> 8 </decimal>
     <fieldwidth> 12 </fieldwidth>
   </format>
   <label> Change </label>
</showvalue>

<showvalue>
   <row> 15.4 </row><col> 1 </col>
   <name> TypeII(%) </name>
   <format><decimal> 1 </decimal></format>
   <label> Type II (%) </label>
</showvalue>

<showvalue>
   <row> 16.4 </row><col> 3 </col>
   <name> TypeIIChange  </name>
   <format>
     <decimal> 8 </decimal>
     <fieldwidth> 12 </fieldwidth>
   </format>
   <label> Change </label>
</showvalue>

<showvalue>
   <row> 17.8 </row><col> 1 </col>
   <name> K12 </name>
   <format>
     <decimal> 8 </decimal>
     <fieldwidth> 12 </fieldwidth>
   </format>
   <label> K12 </label>
</showvalue>

<showvalue>
   <row> 18.8 </row><col> 3 </col>
   <name> K12Basic </name>
   <format>
     <decimal> 8 </decimal>
     <fieldwidth> 12 </fieldwidth>
   </format>
   <label> Basic </label>
</showvalue>

<showvalue>
   <row> 19.8 </row><col> 3 </col>
   <name> K12Multiplier </name>
   <format><decimal> 2 </decimal></format>
   <label> Activity Factor </label>
</showvalue>

<showvalue>
   <row> 21.2 </row><col> 1 </col>
   <name> K21 </name>
   <format>
     <decimal> 8 </decimal>
     <fieldwidth> 12 </fieldwidth>
   </format>
   <label> K21 </label>
</showvalue>

<showvalue>
   <row> 22.2 </row><col> 3 </col>
   <name> K21Basic </name>
   <format>
     <decimal> 8 </decimal>
     <fieldwidth> 12 </fieldwidth>
   </format>
   <label> Basic </label>
</showvalue>

<showvalue>
   <row> 23.2 </row><col> 3 </col>
   <name> K21Multiplier </name>
   <format><decimal> 2 </decimal></format>
   <label> Activity Factor </label>
</showvalue>



===============================================

<showclock>
  <row> 0.5 </row><col> 28 </col>
  <name> System.X </name>
  <timebase> DAY </timebase>
  <days/>
</showclock>

<showgraph>
  <row> 2.0 </row><col> 28 </col>
  <high> 12 </high><wide> 32 </wide>
  <leftmargin> 5 </leftmargin>

  <xaxis>
     <name> System.X </name>
     <label> Minutes </label>
     <scale><min> 0 </min><max> 60 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> MuscleMass </name>
      <label> Mass </label>
      <linecolor> BLUE </linecolor>
    </yvar>
    <scale><min> 0 </min><max> 200 </max></scale>
  </yaxis>
</showgraph>

</panel>
</display>

</model>

EndSpring

Created : 1996-08-29
Last Modified : 2011-06-29
Author : Tom Coleman
Copyright : 2008-2011
By : University of Mississippi Medical Center

A mass is suspended by a spring.  The forces
acting on the mass are due to gravity and the
distension of the spring.

Pull the mass down, let it go, and it will
oscillate up and down.

The independent variable is time.

<?xml version = '1.0' ?>

<model>

<title><basic> Spring </basic></title>

<structure>
<name> Model </name>

<variables> ======================================

The resting length of the spring is the length at
which it produces no force.

<parm>
  <name> RestingLength </name>
  <val> 0 </val>
</parm>

The force produced by the spring is the product of
its stiffness and distention from resting length.

<parm>
  <name> Stiffness </name>
  <val> 10 </val>
</parm>

The mass's upward acceleration is equal to spring
force divided by mass minus the acceleration of
gravity.

<parm>
  <name> Mass </name>
  <val> 1 </val>
</parm>

<parm>
  <name> G </name>
  <val> -9.8 </val>
</parm>

<parm>
  <name> InitialPosition </name>
  <val> 5.0 </val>
</parm>

<var>
  <name> Force </name>
</var>

</variables>

<equations> =====================================

The velocity of the mass is the integral over time
of its acceleration.

<diffeq>
  <name> Velocity </name>
  <integralname> Velocity </integralname>
  <initialval> 0.0 </initialval>
  <dervname> Acceleration </dervname>
  <errorlim> 1E-4 </errorlim>
</diffeq>

The position of the mass is the integral over time
of its velocity.

<diffeq>
  <name> Position </name>
  <integralname> Position </integralname>
  <initialval> 0.0 </initialval>
  <dervname> dPosition/dt </dervname>
  <errorlim> 1E-4 </errorlim>
</diffeq>

</equations>

<definitions> =====================================

<block><name> Parms </name> =====================

<def>
  <name> Position </name>
  <val> InitialPosition </val>
</def>

</block>

<block><name> Dervs </name> =========================================

<def>
  <name> Force </name>
  <val> Stiffness * ( RestingLength - Position ) </val>
</def>

<def>
  <name> Acceleration </name>
  <val> ( Force / Mass ) - G </val>
</def>

<def>
  <name> dPosition/dt </name>
  <val> Velocity </val>
</def>

</block>

</definitions>
</structure>

<math> ===========================================

  <parms> Model.Parms </parms>
  <dervs> Model.Dervs </dervs>

</math>

<control> ========================================

<gofor>
  <solutionint> 5 </solutionint>
  <displayint> 0.1 </displayint>
</gofor>

</control>

<display> =========================================
<panel>

<name> A Spring Model </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<showvalue>
   <row> 2 </row><col> 1 </col>
   <name> System.X </name>
   <format><decimal> 1 </decimal></format>
   <label> Time </label>
</showvalue>

<showvalue>
   <row> 3.5 </row><col> 1 </col>
   <name> Position </name>
   <format><decimal> 1 </decimal></format>
   <label> Position </label>
</showvalue>

<showvalue>
   <row> 4.5 </row><col> 1 </col>
   <name> Velocity </name>
   <format><decimal> 1 </decimal></format>
   <label> Velocity </label>
</showvalue>

<showvalue>
   <row> 5.5 </row><col> 1 </col>
   <name> Acceleration </name>
   <format><decimal> 1 </decimal></format>
   <label> Acceleration </label>
</showvalue>

<groupbox>
   <row> 7.0 </row><col> 1 </col>
   <high> 3.5 </high><wide> 26 </wide>
   <title> Initial Position </title>

<repeatlist>
  <name> InitialPosition </name>
  <firstval> -20 </firstval>
  <repeat>
    <reps> 40 </reps>
    <stepsize> 1 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 1.5 </row><col> 1 </col>
  <name> InitialPosition </name>
  <listname> InitialPosition </listname>
  <label> Position </label>
</slidebar>

</groupbox>

<repeatlist>
  <name> Stiffness </name>
  <repeat>
    <reps> 20 </reps>
    <stepsize> 1 </stepsize>
  </repeat>
  <repeat>
    <reps> 8 </reps>
    <stepsize> 10 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 11.6 </row><col> 1 </col>
  <name> Stiffness </name>
  <listname> Stiffness </listname>
  <label> Stiffness </label>
</slidebar>

<repeatlist>
  <name> Mass </name>
  <repeat>
    <reps> 20 </reps>
    <stepsize> 0.1 </stepsize>
  </repeat>
  <repeat>
    <reps> 8 </reps>
    <stepsize> 1.0 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 13.0 </row><col> 1 </col>
  <name> Mass </name>
  <listname> Mass </listname>
  <label> Mass </label>
</slidebar>

<infobutton>
  <row> 14.6 </row><col> 1 </col>
  <label> Math </label>
  <line> Position = integral ( Velocity )	dt </line>
  <line> Velocity = integral ( Acceleration ) dt </line>
  <line> Acceleration = Force / Mass - Gravity </line>
  <line> Force = Stiffness * ( Position - Resting Length ) </line>
</infobutton>

Right-hand Side ====================================

<showgraph>
  <row> 0.5 </row><col> 30 </col>
  <high> 6 </high><wide> 28 </wide>
  <leftmargin> 10 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 10 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> Position </name>
      <label> Position </label>
      <linecolor> RED </linecolor>
    </yvar>
    <scale><min> -10 </min><max> 10 </max><inc> 10 </inc></scale>
  </yaxis>
</showgraph>

<showgraph>
  <row> 6.9 </row><col> 30 </col>
  <high> 6 </high><wide> 28 </wide>
  <leftmargin> 10 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 10 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> Velocity </name>
      <label> Velocity </label>
      <linecolor> BLUE </linecolor>
    </yvar>
    <scale><min> -20 </min><max> 20 </max><inc> 10 </inc></scale>
  </yaxis>
</showgraph>

<showgraph>
  <row> 13.3 </row><col> 30 </col>
  <high> 6 </high><wide> 28 </wide>
  <leftmargin> 10 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 10 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> Acceleration </name>
      <label> Acceleration </label>
      <linecolor> MAGENTA </linecolor>
    </yvar>
    <scale><min> -60 </min><max> 60 </max><inc> 20 </inc></scale>
  </yaxis>
</showgraph>

</panel>
</display>

</model>

End
Suga - Sagawa Heart

Created : 29-Aug-96
Last Modified : 02-Dec-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

<?xml version = '1.0' ?>

<model>

<title><basic> Suga - Sagawa Heart Model </basic></title>

<structure>
<name> Model </name>

<variables>

The Pericardium

<parm>
  <name> PCVol </name>
  <val> 0 </val>
</parm>

Pulmonary Veins

<parm>
  <name> VP </name>
  <val> 9.0 </val>
</parm>

<parm>
  <name> VenCond </name>
  <val> 1000.0 </val>
</parm>

Thoracic Pressure

<parm>
  <name> PThorax </name>
  <val> -4.0 </val>
</parm>

Systemic Hemodynamics

<parm>
  <name>TPR  </name>
  <val> 0.0185 </val>
</parm>

The left ventricle diastolic P-V curve
is an exponential.

<parm>
  <name> Flaccid </name>
  <val> 0.12 </val>
</parm>

<parm>
  <name> MaxVol </name>
  <val> 200 </val>
</parm>

Left Ventricle Systole

<parm>
  <name> EMax </name>
  <val> 2.0 </val>
</parm>

Cardiac Output Wrapup

<parm>
  <name> HR </name>
  <val> 72 </val>
</parm>

<var>
  <name> PCPres </name>
</var>

<var>
  <name> PCEff </name>
</var>

<var>
  <name> AP </name>
</var>

<var>
  <name> EDP </name>
</var>

<var>
  <name> TMuralP </name>
</var>

<var>
  <name> K </name>
</var>

<var>
  <name> EDV </name>
</var>

<var>
  <name> ESV </name>
</var>

<var>
  <name> SV </name>
</var>

<var>
  <name> EjectionFraction </name>
</var>

</variables>

<equations> ====================================

There is an implicit equation here of
the form CO = f (CO) since CO is both
flow into and flow out of the heart.

<impliciteq>
  <name> CO </name>
  <startname> CO </startname>
  <initialval> 5400.0 </initialval>
  <endname> COEnd </endname>
  <errorlim> 54.0 </errorlim>
  <searchminname> SearchMin </searchminname>
</impliciteq>

</equations>

<functions> ====================================

<curve>
  <name> PCEffCurve </name>
  <point>
    <x> 0 </x><y> 1.0 </y><slope> 0.0 </slope>
  </point>
  <point>
    <x> 20.0 </x><y> 5.0 </y><slope> 0.3 </slope>
  </point>
</curve>

</functions>

<definitions> ===================================

<block><name> Dervs </name> =====================

<def>
  <name> PCPres </name>
  <val> 0.0008 * ( PCVol * PCVol ) </val>
</def>

<def>
  <name> PCEff </name>
  <val> PCEffCurve [ PCPres ] </val>
</def>

<def>
  <name> SearchMin </name>
  <val> 0.0 </val>
</def>

<implicitmath><name> CO </name>

<def>
  <name> AP </name>
  <val> CO * TPR </val>
</def>

<def>
  <name> EDP </name>
  <val> VP - ( CO / VenCond ) </val>
</def>

<def>
  <name> TMuralP </name>
  <val> EDP - PThorax </val>
</def>

<def>
  <name> K </name>
  <val> - ( Flaccid / PCEff ) * TMuralP </val>
</def>

<def>
  <name> EDV </name>
  <val> MaxVol * ( 1 - EXP K ) </val>
</def>

<def>
  <name> ESV </name>
  <val> AP / EMax </val>
</def>

<def>
  <name> SV </name>
  <val> EDV - ESV </val>
</def>

<def>
  <name> COEnd </name>
  <val> HR * SV </val>
</def>

</implicitmath>

<def>
  <name> EjectionFraction </name>
  <val> SV / EDV </val>
</def>

<if>
  <test> AP LE 50 </test>
  <true>
    <page> Severe hypotension. </page>
  </true>
</if>

<if>
  <test> AP GE 150 </test>
  <true>
    <page> Acute hypertension. </page>
  </true>
</if>

</block>

</definitions>
</structure>

<math>
  <dervs> Model.Dervs </dervs>
</math>

<display> =========================================
<panel>

<name> Suga - Sagawa Heart Model </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<showvalue>
   <row> 2 </row><col> 1 </col>
   <name> CO </name>
   <format> <integer/> </format>
   <label> CO </label>
</showvalue>

<showvalue>
   <row> 3 </row><col> 1 </col>
   <name> SV </name>
   <format> <integer/> </format>
   <label> SV </label>
</showvalue>

<showvalue>
   <row> 4 </row><col> 1 </col>
   <name> HR </name>
   <format> <integer/> </format>
   <label> HR </label>
</showvalue>

<showvalue>
   <row> 5.5 </row><col> 1 </col>
   <name> EDV </name>
   <format> <integer/> </format>
   <label> EDV </label>
</showvalue>

<showvalue>
   <row> 6.5 </row><col> 1 </col>
   <name> ESV </name>
   <format> <integer/> </format>
   <label> ESV </label>
</showvalue>

<showvalue>
   <row> 8 </row><col> 1 </col>
   <name> EjectionFraction </name>
   <format><decimal> 2 </decimal></format>
   <label> EjectionFraction </label>
</showvalue>

<repeatlist>
  <name> VP </name>
  <repeat>
    <reps> 30 </reps>
    <stepsize> 1 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 9.5 </row><col> 1 </col>
  <name> VP </name>
  <listname> VP </listname>
  <label> VP </label>
</slidebar>

<showvalue>
   <row> 10.9 </row><col> 1 </col>
   <name> EDP </name>
   <format><decimal> 1 </decimal></format>
   <label> EDP </label>
</showvalue>

<showvalue>
   <row> 11.9 </row><col> 1 </col>
   <name> TMuralP </name>
   <format><decimal> 1 </decimal></format>
   <label> TMuralP </label>
</showvalue>

<showvalue>
   <row> 12.9 </row><col> 1 </col>
   <name> AP </name>
   <format> <integer/> </format>
   <label> AP </label>
</showvalue>

<showvalue>
   <row> 14.4 </row><col> 1 </col>
   <name> VenCond </name>
   <format> <integer/> </format>
   <label> VenCond </label>
</showvalue>

<showvalue>
   <row> 15.4 </row><col> 1 </col>
   <name> TPR </name>
   <format><decimal> 4 </decimal></format>
   <label> TPR </label>
</showvalue>

<infobutton>
  <row> 17.2 </row><col> 1 </col>
  <label> Normal Values </label>
  <line> CO - 5240 mL / Min </line>
  <line> SV - 73 mL </line>
  <line> HR - 72 Beats / Min </line>
  <line>  </line>
  <line> EDV - 121 mL </line>
  <line> ESV - 48 mL </line>
  <line>  </line>
  <line> EjectionFraction - 0.60 </line>
  <line>  </line>
  <line> EDP - 3.8 mmHg </line>
</infobutton>

Right-hand Side ===============================

<infobutton>
  <row> 0.5 </row><col> 30 </col>
  <label> General Math </label>
  <line> CO - Cardiac Output </line>
  <line> EDV - End-Diastolic Volume </line>
  <line> ESV - End-Systolic Volume </line>
  <line> HR - Heart Rate </line>
  <line> SV - Stroke Volume </line>
  <line>  </line>
  <line> CO = HR * SV </line>
  <line> SV = EDV - ESV </line>
</infobutton>

Systole

<groupbox>
   <row> 2.0 </row><col> 30 </col>
   <high> 5.0 </high><wide> 30 </wide>
   <title> Systole </title>

<repeatlist>
  <name> EMax </name>
  <repeat>
    <reps> 50 </reps>
    <stepsize> 0.1 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 1.5 </row><col> 1 </col>
  <name> EMax </name>
  <listname> EMax </listname>
  <label> EMax </label>
</slidebar>

<infobutton>
  <row> 3.2 </row><col> 1 </col>
  <label> Systolic Math </label>
  <line> AP - Arterial Pressure </line>
  <line> CO - Cardiac Output </line>
  <line> EMax - Contractility </line>
  <line> ESV - End-Systolic Volume </line>
  <line> TPR - Total Peripheral Resistance </line>
  <line>  </line>
  <line> AP = CO * TPR </line>
  <line> ESV = AP / EMax </line>
</infobutton>

</groupbox>

Diastole

<groupbox>
   <row> 7.4 </row><col> 30 </col>
   <high> 5.0 </high><wide> 30 </wide>
   <title> Diastole </title>

<repeatlist>
  <name> Flaccid </name>
  <repeat>
    <reps> 50 </reps>
    <stepsize> 0.01 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 1.5 </row><col> 1 </col>
  <name> Flaccid </name>
  <listname> Flaccid </listname>
  <label> Compliance </label>
</slidebar>

<infobutton>
  <row> 3.2 </row><col> 1 </col>
  <label> Diastolic Math </label>
  <line> EDP - End-Diastolic Pressure </line>
  <line> EDV - End-Diastolic Volume </line>
  <line> K - Diastolic Complicance </line>
  <line> MaxVol - Maximum EDV </line>
  <line> PThorax - Thoracic Pressure </line>
  <line> TMuralP - Transmural Pressure </line>
  <line>  </line>
  <line> EDP = f ( filling ) </line>
  <line> TMuralP = EDP - PThorax </line>
  <line> K = f ( Wall Complicance and Pericardial Compression ) </line>
  <line> EDV = MaxVol * ( 1 - EXP ( -K * TMuralP )) </line>
</infobutton>

</groupbox>

Pericardium

<groupbox>
   <row> 12.8 </row><col> 30 </col>
   <high> 4.5 </high><wide> 30 </wide>
   <title> Pericardium </title>

<repeatlist>
  <name> PCVol </name>
  <repeat>
    <reps> 20 </reps>
    <stepsize> 25 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 1.5 </row><col> 1 </col>
  <name> PCVol </name>
  <listname> PCVol </listname>
  <label> PCVol </label>
</slidebar>

<showvalue>
   <row> 2.9 </row><col> 1 </col>
   <name> PCPres </name>
   <format><decimal> 1 </decimal></format>
   <label> Pressure </label>
</showvalue>

</groupbox>

Thorax

<groupbox>
   <row> 17.7 </row><col> 30 </col>
   <high> 3.2 </high><wide> 30 </wide>
   <title> Thorax </title>

<repeatlist>
  <name> PThorax </name>
  <repeat>
    <reps> 20 </reps>
    <stepsize> 1 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 1.5 </row><col> 1 </col>
  <name> PThorax </name>
  <listname> PThorax </listname>
  <label> Pressure </label>
</slidebar>

</groupbox>

</panel>
</display>

</model>

End
Time Delay

Created : 25-Aug-96
Last Modified : 02-Dec-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

A disturbance is not initially propagated from
input to output.  As time passes, the output
gradually increases to equal the input.

A time delay is defined by

   dOutput/dt = K * (Input - Output)

K is the rate constant of the time delay.  The
time constant (Tau) of the delay is the reciprocal
of the rate constant.

   Tau = 1 / K

For a step change in input, when time = Tau,
O = 0.63 * I.

The half-time of the step response is the time
required for the output go one-half way to its
final value.

   t 1/2 = 0.69 * Tau

<?xml version = '1.0' ?>

<model>

<title><basic> Time Delay </basic></title>

<structure>
<name> Model </name>

<variables> =======================================

<parm>
  <name> K </name>
  <val> 0.2 </val>
</parm>

<parm>
  <name> Amp </name>
  <val> 1 </val>
</parm>

<parm>
  <name> At </name>
  <val> 4 </val>
</parm>

<var>
  <name> Tau </name>
</var>

<var>
  <name> t1/2 </name>
</var>

<var>
  <name> I </name>
</var>

</variables>

<equations> ======================================

<diffeq>
  <name> O </name>
  <integralname> O </integralname>
  <initialval>0.0  </initialval>
  <dervname> dO/dt </dervname>
</diffeq>

</equations>

<definitions> ====================================

<block><name> Parms </name> ======================

<def>
  <name> Tau </name>
  <val> INVERT K </val>
</def>

<def>
  <name> t1/2 </name>
  <val> 0.69 * Tau </val>
</def>

</block>

<block><name> Dervs </name> ======================

<conditional>
  <name> I </name>
  <test> System.X GE At </test>
  <true> Amp </true>
  <false> 0 </false>
</conditional>

<def>
  <name> dO/dt </name>
  <val> K * ( I - O ) </val>
</def>

</block>


</definitions>
</structure>

<math>

  <parms> Model.Parms </parms>
  <dervs> Model.Dervs </dervs>

</math>

<control> ========================================

<gofor>
  <solutionint> 10 </solutionint>
  <displayint> 0.5 </displayint>
</gofor>

<gofor>
  <solutionint> 20 </solutionint>
  <displayint> 0.5 </displayint>
</gofor>

</control>

<display> =========================================

<panel><name> Time Delay </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<infobutton>
  <row> 0.5 </row><col> 13 </col>
  <label> The Equation </label>
  <line> The time-delay equation is </line>
  <line>  </line>
  <line> dO / dt = K * (I - O) </line>
  <line>  </line>
  <line> where I is input and O is output. </line>
</infobutton>

<infobutton>
  <row> 0.5 </row><col> 28 </col>
  <label> Rate Constant </label>
  <line> K is the rate constant in </line>
  <line> the time delay equation </line>
  <line>  </line>
  <line> dO / dt = K * (I - O) </line>
  <line>  </line>
  <line> The time constant (Tau) of </line>
  <line> the delay is the reciprocal </line>
  <line> of the rate constant. </line>
  <line>  </line>
  <line> Tau = 1 / K </line>
  <line>  </line>
  <line> For a step change in input, </line>
  <line> when time is one time constant </line>
  <line> beyond the step, the output is </line>
  <line> 63% of the input. </line>
  <line>  </line>
  <line> The half-time of the step </line>
  <line> response is the time required </line>
  <line> for the output to go one-half </line>
  <line> way to its final value. </line>
  <line>  </line>
  <line> t 1/2 = 0.69 * Tau </line>
</infobutton>

<showvalue>
   <row> 3 </row><col> 1 </col>
   <name> I </name>
   <format><decimal> 1 </decimal></format>
   <label> Input </label>
</showvalue>

<showvalue>
   <row> 4 </row><col> 1 </col>
   <name> O </name>
   <format><decimal> 1 </decimal></format>
   <label> Output </label>
</showvalue>

<showvalue>
   <row> 5 </row><col> 1 </col>
   <name> dO/dt </name>
   <format><decimal> 2 </decimal></format>
   <label> dO/dt </label>
</showvalue>

<groupbox>
   <row> 6.6 </row><col> 1 </col>
   <high> 4.5 </high><wide> 26 </wide>
   <title> Step Input </title>

<repeatlist>
  <name> Amp </name>
  <repeat>
    <reps> 5 </reps>
    <stepsize> 1 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 1.5 </row><col> 1 </col>
  <name> Amp </name>
  <listname> Amp </listname>
  <label> Size </label>
</slidebar>

<repeatlist>
  <name> At </name>
  <repeat>
    <reps> 20 </reps>
    <stepsize> 1 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 3.0 </row><col> 1 </col>
  <name> At </name>
  <listname> At </listname>
  <label> At </label>
</slidebar>

</groupbox>

<groupbox>
   <row> 11.9 </row><col> 1 </col>
   <high> 5.5 </high><wide> 26 </wide>
   <title> Rate Constant </title>

<repeatlist>
  <name> K </name>
  <firstval> 0.01 </firstval>
  <repeat>
    <reps> 19 </reps>
    <stepsize> 0.01 </stepsize>
  </repeat>
  <repeat>
    <reps> 8 </reps>
    <stepsize> 0.1 </stepsize>
  </repeat>
</repeatlist>

<slidebar>
  <row> 1.5 </row><col> 1 </col>
  <name> K </name>
  <listname> K </listname>
  <label> Size </label>
</slidebar>

<showvalue>
   <row> 3.0 </row><col> 1 </col>
   <name> Tau </name>
   <format><decimal> 1 </decimal></format>
   <label> Time Constant </label>
</showvalue>

<showvalue>
   <row> 4.0 </row><col> 1 </col>
   <name> t1/2 </name>
   <format><decimal> 1 </decimal></format>
   <label> Half-Life </label>
</showvalue>

</groupbox>

Right-hand Side ==================================

<showvalue>
   <row> 0.5 </row><col> 45 </col>
   <name> System.X </name>
   <format><decimal> 1 </decimal></format>
   <label> Time </label>
</showvalue>

<showgraph>
  <row> 2.5 </row><col> 28 </col>
  <high> 10 </high><wide> 32 </wide>
  <leftmargin> 6 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 20 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> O </name>
      <label> O </label>
      <linecolor> BLUE </linecolor>
    </yvar>
    <yvar>
      <name> I </name>
      <label> I </label>
      <linecolor> RED </linecolor>
    </yvar>
    <scale><min> 0 </min><max> 1 </max></scale>
  </yaxis>
</showgraph>

<showgraph>
  <row> 13 </row><col> 28 </col>
  <high> 6 </high><wide> 32 </wide>
  <leftmargin> 6 </leftmargin>

  <xaxis>
    <name> System.X </name>
    <label> Time </label>
    <scale><min> 0 </min><max> 20 </max></scale>
  </xaxis>

  <yaxis>
    <yvar>
      <name> dO/dt </name>
      <label> dO/dt </label>
    </yvar>
    <scale><min> -0.10 </min><max> 0.2 </max><inc> 0.1 </inc></scale>
  </yaxis>
</showgraph>

</panel>
</display>

</model>

End
Windkessel

Created : 08-Aug-96
Last Modified : 29-Nov-08
Author : Tom Coleman
Copyright : 2008-2009
By : University of Mississippi Medical Center
Solver : Digital Human V0.4
Schema : DES V1.0

<?xml version = '1.0' ?>

<model>

<title><basic> Windkessel </basic></title>

<structure>
<name> Model </name>

<variables>

<parm><name> HR         </name><val> 72   </val></parm>
<parm><name> Comp       </name><val> 1.4  </val></parm>
<parm><name> V0         </name><val> 800  </val></parm>
<parm><name> SystolicK  </name><val> 0.24 </val></parm>
<parm><name> DiastolicK </name><val> 0.01 </val></parm>
<parm><name> FordCond   </name><val> 70   </val></parm>
<parm><name> BackCond   </name><val> 0    </val></parm>
<parm><name> VenPres    </name><val> 7    </val></parm>
<parm><name> PeriCond   </name><val> 0.97 </val></parm>

<var><name> SBP </name><val> UNDEFINED </val></var>
<var><name> DBP </name><val> UNDEFINED </val></var>

<var><name> VPIso          </name></var>
<var><name> AP             </name></var>
<var><name> DelP           </name></var>
<var><name> VelK           </name></var>
<var><name> ValveCond      </name></var>
<var><name> InFlow         </name></var>
<var><name> OutFlow        </name></var>
<var><name> PulsePressure </name></var>

</variables>

<equations>

<diffeq>
  <name> Vol </name>
  <integralname> Vol </integralname>
  <initialval> 925.0 </initialval>
  <dervname> dV/dt </dervname>
  <errorlim> 10.0 </errorlim>
</diffeq>

<impliciteq>
  <name> VP </name>
  <startname> VP </startname>
  <initialval> 0.0 </initialval>
  <endname> VPEnd </endname>
  <errorlim> 1.0 </errorlim>
</impliciteq>

</equations>

<definitions> =======================================================

<block><name> Dervs </name> =========================================

<def>
  <name> AP </name>
  <val> ( 1 / Comp ) * ( Vol - V0 ) </val>
</def>

<def>
  <name> VPIso </name>
  <val> 200 * SIN ( 2 * System.Pi * ( HR / 60 ) * System.X ) MAX 0 </val>
</def>

<implicitmath><name> VP </name>

<def>
  <name> DelP </name>
  <val> VP - AP </val>
</def>

<conditional>
  <name> VelK </name>
  <test> DelP GT 0 </test>
  <true> SystolicK </true>
  <false> DiastolicK </false>
</conditional>

<conditional>
  <name> ValveCond </name>
  <test> DelP GT 0 </test>
  <true> FordCond </true>
  <false> BackCond </false>
</conditional>

<def>
  <name> InFlow </name>
  <val> DelP * ValveCond </val>
</def>

<def>
  <name> VPEnd </name>
  <val> VPIso - ( VelK * InFlow ) </val>
</def>

</implicitmath>

<def>
  <name> OutFlow </name>
  <val> ( AP - VenPres ) * PeriCond </val>
</def>

<def>
  <name> dV/dt </name>
  <val> InFlow - OutFlow </val>
</def>

Track max and min AP for ABP and DBP.

<if><test> System.X EQ 0 </test>
<true>
  <def><name> SBP </name><val> AP </val></def>
  <def><name> DBP </name><val> AP </val></def>
</true>

<false>

  <andif><test> AP GT SBP </test>
  <true>
    <def><name> SBP </name><val> AP </val></def>
  </true>
  </andif>

  <andif><test> AP LT DBP </test>
  <true>
    <def><name> DBP </name><val> AP </val></def>
  </true>
  </andif>

</false>
</if>

Pulse pressure for display.

<def><name> PulsePressure </name><val> SBP - DBP </val></def>

</block>

</definitions>
</structure>

<math>

  <dervs> Model.Dervs </dervs>

</math>

<control> ========================================

<gofor>
  <solutionint> 1 </solutionint>
  <displayint> 0.05 </displayint>
</gofor>

<gofor>
  <solutionint> 2 </solutionint>
  <displayint> 0.05 </displayint>
</gofor>

<gofor>
  <solutionint> 4 </solutionint>
  <displayint> 0.1 </displayint>
</gofor>

</control>

<display> =========================================
<panel>

<name> Windkessel Model </name>

<structurename> Model </structurename>

<showpanelname>
  <row> 0.5 </row><col> 1.0 </col>
</showpanelname>

<showvalue>
   <row> 2.0 </row><col> 1.0 </col>
   <name> VPIso </name>
   <format> <integer/> </format>
   <label> Isometric VP </label>
</showvalue>

<showvalue>
   <row> 3.0 </row><col> 1.0 </col>
   <name> VP </name>
   <format> <integer/> </format>
   <label> Ventricular Pressure </label>
</showvalue>

<showvalue>
   <row> 4.0 </row><col> 1.0 </col>
   <name> AP </name>
   <format> <integer/> </format>
   <label> Arterial Pressure </label>
</showvalue>

<showvalue>
   <row> 5.5 </row><col> 1.0 </col>
   <name> SBP </name>
   <format> <integer/> </format>
   <label> SBP </label>
</showvalue>

<showvalue>
   <row> 6.5 </row><col> 1.0 </col>
   <name> DBP </name>
   <format> <integer/> </format>
   <label> DBP </label>
</showvalue>

<showvalue>
   <row> 7.5 </row><col> 1.0 </col>
   <name> PulsePressure </name>
   <format> <integer/> </format>
   <label> Pulse Pressure </label>
</showvalue>

<showvalue>
   <row> 9.0 </row><col> 1.0 </col>
   <name> InFlow </name>
   <format> <integer/> </format>
   <label> In Flow </label>
</showvalue>

<showvalue>
   <row> 10.0 </row><col> 1.0 </col>
   <name> OutFlow </name>
   <format> <integer/> </format>
   <label> Out Flow </label>
</showvalue>

Valve Conductance

<groupbox>
   <row> 11.5 </row><col> 1.0 </col>
   <high> 4.5 </high><wide> 30 </wide>
   <title> Valve Conductance </title>

<repeatlist>
  <name> FordCond </name>
  <repeat><reps> 20 </reps><stepsize> 10 </stepsize></repeat>
</repeatlist>

<slidebar>
  <row> 1.5 </row><col> 1.0 </col>
  <name> FordCond </name>
  <listname> FordCond </listname>
  <label> Forward </label>
</slidebar>

<repeatlist>
  <name> BackCond </name>
  <repeat><reps> 20 </reps><stepsize> 0.5 </stepsize></repeat>
</repeatlist>

<slidebar>
  <row> 3.0 </row><col> 1.0 </col>
  <name> BackCond </name>
  <listname> BackCond </listname>
  <label> Backward </label>
</slidebar>

</groupbox>

Windkessel

<groupbox>
   <row> 16.4 </row><col> 1.0 </col>
   <high> 5.5 </high><wide> 30 </wide>
   <title> Windkessel </title>

<repeatlist>
  <name> Comp </name>
  <repeat><reps> 30 </reps><stepsize> 0.1 </stepsize></repeat>
</repeatlist>

<slidebar>
  <row> 1.5 </row><col> 1.0 </col>
  <name> Comp </name>
  <listname> Comp </listname>
  <label> Compliance </label>
</slidebar>

<showvalue>
   <row> 2.9 </row><col> 1.0 </col>
   <name> Vol </name>
   <format> <integer/> </format>
   <label> Volume </label>
</showvalue>

<showvalue>
   <row> 3.9 </row><col> 1.0 </col>
   <name> V0 </name>
   <format> <integer/> </format>
   <label> Unstressed Volume </label>
</showvalue>

</groupbox>

Info Stack

<infobutton>
  <row> 23.0 </row><col> 1.0 </col>
  <label> Ventricle Math </label>
  <line> HR - Heart Rate </line>
  <line> InFlow - Ventricle OutFlow and Windkessel Inflow </line>
  <line> K - Velocity Constant </line>
  <line> VP - Ventricular Pressure </line>
  <line> VPIso - Isometric VP </line>
  <line> VPMax - Maximum VP </line>
  <line></line>
  <line> Sin = sin ( 2 * PI * ( HR / 60 ) * Time ) </line>
  <line> VPIso = VPMax * Sin for Sin >= 0 </line>
  <line> VP = VPIso - K * Inflow </line>
</infobutton>

<infobutton>
  <row> 24.6 </row><col> 1.0 </col>
  <label> Valve Math </label>
  <line> AP - Arterial Pressure </line>
  <line> BackCond - Backward Valve Conduct </line>
  <line> Cond - Current Valve Conductance </line>
  <line> DelP - Valve Pressure Gradient </line>
  <line> FordCond - Forward Valve Conductance </line>
  <line> InFlow - Ventricle OutFlow and Windkessel Inflow </line>
  <line> VP - Ventricular Pressure </line>
  <line></line>
  <line> DelP = VP - AP </line>
  <line> if ( DelP > 0 ) </line>
  <line> Cond = FordCond </line>
  <line> else </line>
  <line> Cond = BackCond </line>
  <line> InFlow = DelP * Cond </line>
</infobutton>

<infobutton>
  <row> 26.2 </row><col> 1.0 </col>
  <label> Windkessel Math </label>
  <line> AP - Arterial Pressure </line>
  <line> C - Compliance </line>
  <line> Vol - Arterial Volume </line>
  <line> V0 - Unstressed Volume </line>
  <line></line>
  <line> AP = ( 1 / C ) * ( Vol - V0 ) </line>
</infobutton>

<infobutton>
  <row> 27.8 </row><col> 1.0 </col>
  <label> Periphery Math </label>
  <line> AP - Arterial or Windkessel Pressure </line>
  <line> Cond - Peripheral Conductance </line>
  <line> OutFlow - Windkessel Outflow </line>
  <line></line>
  <line> OutFlow = ( AP - VP ) * Cond </line>
</infobutton>

Right-hand Side --------------------------------------------

<showvalue>
   <row> 0.5 </row><col> 35.0 </col>
   <name> System.X </name>
   <format><decimal> 2 </decimal></format>
   <label> Time (Sec) </label>
</showvalue>

<showgraph>
  <row> 2.0 </row><col> 32.0 </col>
  <high> 14 </high><wide> 28 </wide><leftmargin> 7 </leftmargin>
  <xaxis>
     <name> System.X </name>
     <label> Time </label>
     <scale><min> 0 </min><max> 2 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> InFlow </name>
      <label> In Flow </label>
      <linetype> DOT </linetype>
    </yvar>
    <yvar>
      <name> VP </name>
      <label> VP </label>
      <linecolor> RED </linecolor>
    </yvar>
    <yvar>
      <name> AP </name>
      <label> AP </label>
      <linecolor> BLUE </linecolor>
    </yvar>
    <scale><min> 0 </min><max> 500 </max><inc> 100 </inc></scale>
  </yaxis>
</showgraph>

<showgraph>
  <row> 16.4 </row><col> 32.0 </col>
  <high> 7 </high><wide> 28 </wide><leftmargin> 7 </leftmargin>
  <xaxis>
     <name> System.X </name>
     <label> Time </label>
     <scale><min> 0 </min><max> 2 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> InFlow </name>
      <label> In Flow </label>
      <linecolor> RED </linecolor>
    </yvar>
    <yvar>
      <name> OutFlow </name>
      <label> Out Flow </label>
      <linecolor> BLUE </linecolor>
    </yvar>
    <scale><min> 0 </min><max> 500 </max><inc> 100 </inc></scale>
  </yaxis>
</showgraph>

<showgraph>
  <row> 23.8 </row><col> 32.0 </col>
  <high> 5 </high><wide> 28 </wide><leftmargin> 7 </leftmargin>
  <xaxis>
     <name> System.X </name>
     <label> Time </label>
     <scale><min> 0 </min><max> 2 </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> ValveCond </name>
      <label> Valve </label>
    </yvar>
    <scale><min> 0 </min><max> 100 </max></scale>
  </yaxis>
</showgraph>

Show file at bottom.

<showfile>
  <row> 30.0 </row><col> 4.0 </col>
  <wide> 62 </wide>
</showfile>

</panel>
</display>

</model>

End