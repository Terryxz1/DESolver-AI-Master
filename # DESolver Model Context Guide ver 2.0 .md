# DESolver Model Context Guide

## 1. Basic Structure
A DESolver model consists of series of elements nested within each other. 
- Start with the xml declaration. 
- The highest level element is called the 'parent' element. The parent element can contain other elements, called 'child' elements.
 Model is the single highest level element. Structure, math, control and display are all child elements of the model element. 
 Structure contains variables, equations and definitions. 

<?xml version='1.0' ?>

<model>
  <title><basic> Model Name </basic></title>
  
  <structure>
    <name> StructureName </name>
    <variables> <!-- Variable declarations and values --> </variables>
    <equations> <!-- Equations of  --> </equations>
    <definitions> <!-- Blocks with definitions including calculations --> </definitions>
  </structure>

  <math>
    <context> StructureName.Context </context>
    <parms> StructureName.Parms </parms>
    <dervs> StructureName.Dervs </dervs>
    <wrapup> StructureName.Wrapup </wrapup>
  </math>

  <control>
    <gofor>
      <solutionint> Solution Interval </solutionint>
      <displayint> Display Interval </displayint>
    </gofor>
  </control>

  <display>
    <panel>
      <name> Panel Name </name>
      <structurename> StructureName </structurename>
      <!-- Display elements -->
    </panel>
  </display>
</model>

- The above order and nesting of elements must be followed exactly.

## 2. Variable Types
These are the variable types: 
- `<parm>`: User or program-adjustable parameters
- `<var>`: Calculated variables
- `<constant>`: Fixed values
- `<timervar>`: Timer variable, defined in definitions section

- variables are defined either in the definitions section <def> or connected to a display element and subsequently referenced for other calculations.   

## 3. Equation Types 
These are the equation types that can be used in the equations section:
- `<diffeq>`: Differential equations
- `<impliciteq>`: Implicit equations
- `<backwardeuler>`: Backward Euler equations

The only functions that can be used in the equations section are curves.
Equations and Functions need to go into the derivatives blocks as declared in <dervs> in the math section. 
## 4. Mathematical Operations
Note: 
Mathematical operations are solved in their order from left to right. Parenthesis must be used for correct order of calculations for example: A + B * C ^ 2 - in this circumstance the multiplication will come after addition and exponent last. 
( A + B ) * ( C ^ 2 ) - this would yield the expected solution. 

- Basic: +, -, *, /, ^
- Functions: SIN, COS, EXP, LOG, ABS
- Conditionals: GT, LT, GE, LE, EQ, AND, OR
- Special: INVERT (1/x), MAX, MIN, 
- Other Numbers: System.Pi, System.X, 
The last two are Pi and independent variable X or time 

Note: There must be space before and after parentheses or any basic operators to be apropriately tokenized by the parser. For example: ( A + B ) * ( C ^ 2 ) * - 5

## 6. Equations Section
Note:
- IMPORTANT: All equations within the top-level element <equations> are self declared. This means that <integralname> must be a unique name and there is no need to declare it as a  <var> in the variables section. 
- Equations must have a numerical value entered for Error Limits and Initial Values. 
Dervname is declared in the equations and then defined in the definitions section. 

### 5.1 Differential Equations
Used for standard differential equations that are not stiff.

<diffeq>
  <name> AnyName </name>
  <integralname> VariableName </integralname>
  <initialval> Initial Value </initialval>
  <dervname> DerivativeName </dervname>
  <errorlim> Error Limit </errorlim>
</diffeq>

### 6.2 Implicit Equations
Used for solving algebraic constraints of the form Y = f(Y).

<impliciteq>
  <name> VariableName </name>
  <errorlim> Error Limit </errorlim>
  <leftside> Expression1 </leftside>
  <rightside> Expression2 </rightside>
</impliciteq>

### 6.3 Backward Euler Equations
Used for stiff equations where standard integration methods may be unstable.

<backwardeuler>
  <name> AnyName </name>
  <integralname> VariableName </integralname>
  <initialval> Initial Value </initialval>
  <f1name> InputFunction </f1name>
  <f2name> DecayFunction </f2name>
  <dervname> DerivativeName </dervname>
  <errorlim> Error Limit </errorlim>
</backwardeuler>

Note:
- f1name and f2name are defined in the definitions section, with the name of the function matching f1name and f2name, followed by value of the functions. 

### 6.4 Curve Functions

#### Basic Curve
<curve>
  <name> CurveName </name>
  <point><x> x1 </x><y> y1 </y><slope> s1 </slope></point>
  <point><x> x2 </x><y> y2 </y><slope> s2 </slope></point>
  <!-- Additional points -->
</curve>

#### Curve Properties
- Points must be in ascending x order
- Linear interpolation between points
- Extends first/last slopes beyond endpoints
- Slope element is optional

Example:
<!-- Oxygen-Hemoglobin Dissociation Curve -->
<curve>
  <name> O2Saturation </name>
  <point><x> 0 </x><y> 0 </y><slope> 0 </slope></point>
  <point><x> 26.8 </x><y> 50 </y><slope> 1.2 </slope></point>
  <point><x> 100 </x><y> 100 </y><slope> 0 </slope></point>
</curve>

<def>
  <name> SaO2 </name>
  <val> O2Saturation [ PaO2 ] </val>
</def>

## 7 Block Types in Definitions

### 7.1 Basic Block Structure
Blocks are only found in the definitions section. 
Blocks can be called using global naming conventions that uses a period to separate the block name from the structure name. 

Structurename.Blockname 

<block>
  <name> BlockName </name>
  <!-- Calculations within blocks -->
  <def>
    <name> Name </name>
    <val> Value </val>
  </def>
</block>

###  Conditional Elements

#### If Element
<if>
  <test> Evaluate this expression </test>
  <true> Math for TRUE outcome </true>
  <false> Math for FALSE outcome </false>
</if>

Note: Nested if elements are not allowed - use andif for nested conditions.

#### Andif Element
<andif>
  <test> Evaluate this expression </test>
  <true> Math for TRUE outcome </true>
  <false> Math for FALSE outcome </false>
</andif>

Example of nested conditions:
<if>
  <test> Temp GT 38.0 </test>
  <true>
    <andif>
      <test> HR GT 100.0 </test>
      <true> 
        <def><name> Status </name><val> FEVER_TACHYCARDIA </val></def>
      </true>
      <false>
        <def><name> Status </name><val> FEVER_ONLY </val></def>
      </false>
    </andif>
  </true>
  <false>
    <def><name> Status </name><val> NORMAL </val></def>
  </false>
</if>

#### Conditional Element
<conditional>
  <name> Variable to be assigned </name>
  <test> Evaluate this expression </test>
  <true> Expression for TRUE outcome </true>
  <false> Expression for FALSE outcome </false>
</conditional>



## 11. Calculation Order and Math Blocks

### 11.1 Math Block Types
The equation solver performs four types of calculations in order:

1. **Context Math**
   - Calculated once at solution start
   - Used primarily for model scaling
   - Defined in `<context>` block

2. **Parameter Math**
   - Calculated after parameter changes
   - Defines values derived from parameters
   - Defined in `<parms>` block

3. **Derivative Math**
   - Calculated multiple times during each integration interval
   - Core calculations for model dynamics
   - Defined in `<dervs>` block

4. **Wrapup Math**
   - Calculated after successful integration
   - Used for display variables
   - Handles potential discontinuities
   - Defined in `<wrapup>` block

### 11.2 Calculation Sequence Rules

1. **Definition Before Use**
   - Variables must be defined before being referenced
   - Example of incorrect sequence:
   ```xml
   <def>
     <name> A </name>
     <val> B * 2.0 </val>  <!-- Error: B not yet defined -->
   </def>
   <def>
     <name> B </name>
     <val> 5.0 </val>
   </def>
   ```

2. **Block Order**
   - Blocks are calculated in order of appearance
   - Use `<call>` to invoke blocks in specific sequence
   - Blocks can be called within other blocks
   - The top level blocks are listed in the math section and their functions are described in 11.1 Math Block Types. 
   ```xml
   <block>
     <name> FirstBlock </name>
     <!-- calculations -->
   </block>
   
   <block>
     <name> SecondBlock </name>
     <call> FirstBlock </call>  <!-- FirstBlock calculated before SecondBlock -->
     <!-- more calculations -->
   </block>
   ```

3. **Math Section Structure**

   ```xml
   <math>
     <context> Structure.Context </context>
     <parms> Structure.Parms </parms>
     <dervs> Structure.Dervs </dervs>
     <wrapup> Structure.Wrapup </wrapup>
   </math>
   ```
   Note: the above math blocks are used based on necessity. However, dervs is always required. 

### 11.3 Best Practices for Calculation Order

1. **Organize Dependencies**
   - Map variable dependencies
   - Place independent calculations first
   - Group related calculations together

2. **Block Structure**
   ```xml
   <block>
     <name> Calculations </name>
     
     <!-- Independent variables first -->
     <def>
       <name> BaseValue </name>
       <val> 100.0 </val>
     </def>
     
     <!-- Dependent variables next -->
     <def>
       <name> ScaledValue </name>
       <val> BaseValue * Scale </val>
     </def>
     
     <!-- Complex calculations last -->
     <def>
       <name> FinalValue </name>
       <val> ComplexFunction [ ScaledValue ] </val>
     </def>
   </block>
   ```

3. **Common Patterns**
   - Parameters → Scaling → Core Calculations → Display
   - Constants → Variables → Derivatives
   - Base Values → Modifiers → Final Values

## 5. Control Section
Must be included for simulation timing, should include multiple <gofor> elements with apropriate solution intervals. Display intervals should be proportionally, not greater than x20 smaller than solution intervals. 
- System.X is the independent variable on X axis, and each 1 unit is meant to represent 1 minute. 

```xml
<control>
<!-- This example demonstrates 1 minute interval with 10 displayed values and a 10 minute interval with 20 values -->
 <gofor>
    <solutionint> 1.0 </solutionint>
    <displayint> 0.1 </displayint>
  </gofor>

  <gofor>
    <solutionint> 10.0 </solutionint>
    <displayint> 0.5 </displayint>
  </gofor>
</control>
```


## 7. Display Elements

### 7.1 Basic Display Elements
 
<maplist>
  <row> RowNumber </row>
  <col> ColumnNumber </col>
  <name> VariableName </name>
  <label> Display Label </label>
</maplist>

- repeatlist and sliderbar are used together to create a user interface for the parameter values as named in the variables section. 

<slidebar>
  <row> RowNumber </row><col> ColumnNumber </col>
  <name> VariableName </name>
  <listname> VariableName </listname>
  <label> Display Label </label>
</slidebar>

<repeatlist>
  <name> VariableName </name>
  <repeat>
    <reps> Number of Steps </reps>
    <stepsize> Increment </stepsize>
  </repeat>
</repeatlist>

<showvalue>
  <row> RowNumber </row>
  <col> ColumnNumber </col>
  <name> VariableName </name>
  <format><decimal> DecimalPlaces </decimal></format>
  <label> Display Label </label>
</showvalue>

<showgraph>
  <row> RowNumber </row>
  <col> ColumnNumber </col>
  <high> Height </high>
  <wide> Width </wide>
  <leftmargin> MarginSize </leftmargin>
  <xaxis> <!-- X-axis configuration --> </xaxis>
  <yaxis> <!-- Y-axis configuration --> </yaxis>
</showgraph>

### 7.2 Interactive Controls

#### Radio Buttons
<radiobuttons>
  <row> RowNumber </row>
  <col> ColumnNumber </col>
  <name> ParameterName </name>
  <label> Control Label </label>
  <choice>
    <name> Choice1 </name>
    <value> 1 </value>
    <label> Option 1 </label>
  </choice>
  <choice>
    <name> Choice2 </name>
    <value> 2 </value>
    <label> Option 2 </label>
  </choice>
</radiobuttons>

#### Checkboxes
<checkbox>
  <row> RowNumber </row>
  <col> ColumnNumber </col>
  <name> ParameterName </name>
  <label> Checkbox Label </label>
  <true> Value When Checked </true>
  <false> Value When Unchecked </false>
</checkbox>

#### Dropdown Menus
<dropdownmenu>
  <row> RowNumber </row>
  <col> ColumnNumber </col>
  <name> ParameterName </name>
  <label> Menu Label </label>
  <choice>
    <name> Option1 </name>
    <value> 1 </value>
  </choice>
  <choice>
    <name> Option2 </name>
    <value> 2 </value>
  </choice>
</dropdownmenu>

## 8. Best Practices

### 8.1 Display Layout
1. Logical Grouping
   - Group related controls together
   - Use groupbox for visual organization
   - Consider tab organization for complex displays

2. Spacing Guidelines
   - Leave adequate space between elements
   - Align elements for visual clarity
   - Use consistent margins

3. Interactive Element Guidelines
   - Place frequently used controls within easy reach
   - Group related controls together
   - Provide clear labels and instructions

### 8.2 Model Structure
1. Match structurename in panel with structure name
2. Place repeatlist near its corresponding slidebar
3. Use meaningful variable names
4. Include error limits for integration
5. Document with comments
6. Group related displays together

## 9. Common Applications
1. Physical systems
2. Physiological systems
3. Control systems
4. Time-delay systems
5. Multi-compartment models

## 9. Error Handling
1. Check for undefined variables
2. Verify integration error limits
3. Ensure proper equation sequencing
4. Validate parameter ranges
5. Handle division by zero cases

## 10. Required Block Names in Math Section
- context
- parms
- dervs
- wrapup

Note: Block names in definitions can vary, but must match the names used in the math section. 

## 6. Display Organization
Group related elements:
```xml
<groupbox>
  <row> 2.0 </row><col> 1.0 </col>
  <high> 5.0 </high><wide> 30.0 </wide>
  <title> Environmental Controls </title>
  <!-- Controls within group -->
</groupbox>
```

## 7. Best Practices
1. Only include math blocks that are needed
2. Group related variables and controls
3. Use meaningful names
4. Include error limits for integration
5. Document with comments


