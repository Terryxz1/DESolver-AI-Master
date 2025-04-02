# DESolver Model Context Guide

## 1. XML Basics
XML documents use tags (angle brackets) to organize data:
- Tags: <name> and </name>
- Elements: Complete tag-content-tag sequence
- Empty elements: <beep/>
- Attributes: name="value" pairs within tags
- Root element: Single highest-level element (usually <model>)

### Processing Instructions
Special instructions starting with <?:
- `<?xml version='1.0' ?>`
- `<?include filename ?>`
- `<?path pathname ?>`
- `<?create token_name ?>`
- `<?if token_name filename ?>`
- `<?ifnot token_name filename ?>`

### Special Characters
| Character | Symbol | Entity |
|-----------|--------|--------|
| Ampersand | & | &amp; |
| Apostrophe | ' | &apos; |
| Greater Than | > | &gt; |
| Less Than | < | &lt; |
| Quotation Mark | " | &quot; |

## 2. Basic Model Structure

<model>
  <schema> Schema version </schema>
  <modeltitle> Model Title </modeltitle>
  
  <structure>
    <name> Structure Name </name>
    <variables>
      <constant> <!-- Fixed values --> </constant>
      <parm> <!-- User parameters --> </parm>
      <var> <!-- Calculated variables --> </var>
    </variables>
    
    <equations>
      <diffeq>
        <name> Variable </name>
        <integralname> IntegralName </integralname>
        <initialval> Initial Value </initialval>
        <dervname> Derivative Name </dervname>
        <errorlim> Error Limit </errorlim>
      </diffeq>
      
      <impliciteq>
        <name> Variable </name>
        <startname> StartName </startname>
        <initialval> Initial Value </initialval>
        <endname> EndName </endname>
        <errorlim> Error Limit </errorlim>
      </impliciteq>
    </equations>
    
    <definitions>
      <block>
        <name> BlockName </name>
        <def>
          <name> Variable </name>
          <val> Expression </val>
        </def>
      </block>
    </definitions>
  </structure>

  <math>
    <parms> Structure.Parms </parms>
    <dervs> Structure.Dervs </dervs>
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
      <structurename> Structure Name </structurename>
      <!-- Display elements follow -->
    </panel>
  </display>
</model>

## 3. Display Elements

### Value Display
<showvalue>
  <row> Row Number </row>
  <col> Column Number </col>
  <name> Variable Name </name>
  <format><decimal> Decimal Places </decimal></format>
  <label> Display Label </label>
</showvalue>

### Graphs
<showgraph>
  <row> Row Number </row>
  <col> Column Number </col>
  <high> Height </high>
  <wide> Width </wide>
  <xaxis>
    <name> X Variable </name>
    <label> X Label </label>
    <scale><min> Min </min><max> Max </max></scale>
  </xaxis>
  <yaxis>
    <yvar>
      <name> Y Variable </name>
      <label> Y Label </label>
      <linecolor> Color </linecolor>
    </yvar>
    <scale><min> Min </min><max> Max </max></scale>
  </yaxis>
</showgraph>

### User Controls
<slidebar>
  <row> Row Number </row>
  <col> Column Number </col>
  <name> Parameter Name </name>
  <listname> List Name </listname>
  <label> Display Label </label>
</slidebar>

<repeatlist>
  <name> List Name </name>
  <repeat>
    <reps> Number of Steps </reps>
    <stepsize> Step Size </stepsize>
  </repeat>
</repeatlist>

## 4. Mathematical Operations

### Unary Operators
- `-` : Multiply by -1
- `INVERT` : 1.0 divided by
- `EXP` : Exponential function
- `LOG` : Natural logarithm
- `LOG10` : Base 10 logarithm
- `SQRT` : Square root
- `SIN`, `COS`, `TAN` : Trigonometric functions
- `ARCSIN`, `ARCCOS`, `ARCTAN` : Inverse trigonometric
- `ABS` : Absolute value

### Binary Operators
- `+`, `-`, `*`, `/` : Basic arithmetic
- `^` : Power
- `MIN`, `MAX` : Minimum/maximum
- `GT`, `LT`, `GE`, `LE`, `EQ`, `NE` : Comparisons
- `AND`, `OR` : Logical operators

### Important Rules
1. All tokens must be separated by whitespace
2. Calculations inside parentheses are done first
3. No operator precedence - calculations proceed left to right
4. Use parentheses when in doubt

## 5. Best Practices
1. Document files with creation and modification dates:
<!-- datecreated dd-mmm-yy -->
<!-- datelastchanged dd-mmm-yy -->
2. Include error limits for differential equations
3. Use meaningful variable and structure names
4. Group related displays together
5. Document model assumptions and equations
6. Test boundary conditions and stability

## 6. Common Issues
1. Missing whitespace between tokens
2. Undefined variables
3. Division by zero
4. Integration instability
5. Incorrect calculation order
6. Missing error limits

Note: This guide is based on the HumMod Schema documentation. For complete details, refer to the original schema documentation.