java cAdvanced Control (EEEE4116)
Coursework 1
Modelling and Advanced Controller Design for a 2-Level Grid-Feeding Inverter
In this assignment you will bring together your skills of state-space equation development and controller 
design to control a grid-tied 2-Level Converter. The design will make use of transforming the 3-phase 
behaviour of this converter into the dq frame and use the dq equivalent circuit to develop controls. If you 
have not yet read the coursework summary, it is highly recommended you read this prior to get the 
understanding of what dq transforms are and why we are developing a control system in this way. 
Figure 1- Notional System Diagram: DC Source interfaced with 3-phase 2-Level Inverter interfaced to the grid.
The system under investigation is a very common application when trying to link renewable energy sources 
such as solar panels, or energy storage systems to interface them to the national grid, or even microgrid 
applications where small remote communities rely on generating their own power. 
We are converting DC power into 3-phase AC power to connect to the national grid. The parameters which 
will be used in the design is as follows: 
Vdc DC Supply Voltage = 400V
Vgrid Grid RMS Voltage = 230Vrms
L Output Filter Inductance = 470mH
R Intrinsic Filter Resistance = 2Ω
C Output Filter Capacitance = 33uF
ω Grid Frequency (rads-1
) = 100π rads-1
fs
Switching Frequency = 20kHz
Exercise 1 – System Modelling 
As per the coursework summary, we wish to develop our control strategy using the dq equivalent model. It 
can be shown in [ X ] that an equivalent 3-phase inverter can be modelled using the following circuits when 
observing converter dynamics in the dq domain:
Figure 2- 3-Phase Inverter dq equivalent average model.
Where: 
• md: d-axis modulation index. 
• mq: q-axis modulation index. 
• ω: frequency of phase voltages (rads-1
) 
• Vcd / Vcq: d-axis and q-axis voltage respectively across capacitor
• Iid / Iiq: d-axis and q-axis input current respectively from inverter
• Icd / Icq: d-axis current respectively flowing into capacitor.
• Iad / Iaq: d-axis and q-axis output currents after filter. 
• represents a virtual voltage source in the system (due to changing currents in inductor)
• represents a virtual current source in the system (due to changing voltages in capacitor)
Hint: Note the directions of the virtual voltage and current sources. Vital to this exercise.
Using Kirchhoff’s current and voltage laws on the two circuits shown in Figure 2, develop state-equations for d and q 
axis voltage and currents. 
In our system, we will treat the modulation indexes as inputs to our system. Using your state equations, go on to 
show that the state-space equation defining the model can be shown to be as: 
As you may have recognised, although Iad and Iaq are variables in our dq model, this variable is not included within 
the state-equation. Similar could be said about Icd, and Icq. Explain why these terms are not present in the final 
state-space equation. 
In addition, what sources of error do you think could be attributed in the model, and what effect do you think this 
could have on the system?
Exercise 2 – Transfer Function Depiction 
Whilst state-space can describe the system with differential equations, it still does not fully replace the 
transfer function for model development. In fact, often a transfer function block diagram is first developed 
to visualize the behaviours of a system and help formulate state-space models. 
In the first part of this exercise, analyse Fig 2 and construct a transfer block diagram for the above model. A 
template has been provided below to assist you construct the block diagram. Explain in your report how 
you derived the terms in the block diagram.
_
_
Figure 3: Structure of the block diagram for the dq equivalent model. Fill in the blanks and '?' to complete the model.
The boxes with ‘?’ should be transfer functions in Laplace Form. The dashed boxes are interconnections 
between terms to form the mathematical model of the system. 
After constructing the model, in MATLAB enter your state-space system using the ss(A,B,C,D) command. 
Compare the eigenvalues of the state-space representation to that of the transfer functions in Fig 3, and 
can you explain why there is a discrepancy between them?
As shown through the state-space equations, and the transfer function block diagram, the two circuits 
exhibit cross-coupling (sharing of terms in each other’s models). Why is cross coupling an issue in the 
controller design, and how could you augment the transfer block diagram to compensate for cross-coupling 
affects?
Exercise 3 – Control Philosophy 
We have now derived the state-space equations for our inverter, and modelled the transfer function block 
diagram, all within the dq domain. We will now start designing the controller for our inverter. 
We want our converter to be able to respond quickly to any changes in demand from the grid. Therefore, a 
settling time to maintain the peak voltage will be set to 200ms. This in turn will influence the phase change 
of the inverter similarly. 
Decide with explanation the choice of poles being used in the pole-placement algorithm and create 
feedback controller K for our system. You can augment the system by feeding the output of the controller 
directly into the B Matrix as follows:
Figure 4: Basic state-space block diagram to analyse performance of designed controller.
In default condition, you may see the output not change at all. What can you change in the above model to 
evaluate the closed loop performance of the system.
One you can see the dynamics of the outputs, evaluate the response, and reflect if the system is behaving 
according to your design.
You will realise from the system should operate much better than that of open loop, however there is 
currently no ability to attain our desired voltages in this control scheme as we have no references for which 
the controller can actuate upon. If you analyse the output of your controller ‘K’ when using the block 
diagram in Fig 4, what are the outputs of K tending to, and can you explain why the system is tending to 
this value. 
To integrate references into our model, we must introduce integral states. Identify which states you'll 
reference for grid applications. Discuss in your report how these integral states are added to the state space block diagram, and how they help achieve zero steady state error. Given the expanded state system, 
reperform the pole-placement with new poles, justifying your selection while ensuring desired converter 
performance.
In the report, discuss the adaptation to the state-space equation to incorporate integral states, the need for 
them, the design of the new poles for the system. Show, with explanations how the state-space block 
diagram is augmented for the inclusion of integral states. Analyse the performance of the system, especially 
any foreseeable issues you can observe with the system? Prove to the reader that even with the 
adaptation, the system operates still within desired specification. 
Exercise 4 – Integrator Preloading and Integrator Anti windup Implementations
Integrator terms are very influential regarding system performance. It’s not simply that having them in our 
system allows our system to achieve zero steady state error but can also heavily influence the start-up 
performance of any control system, and the step response of a system when there are physics limitations.
Using the working closed loop model with integral states, place a scope on each of the integrator outputs
(that for the states as well as integral states) and the system outputs. 
Figure 5: Placement of Scope on Output of integrator block
What do you observe in the relationship between the integrator outputs and the system outputs?
A solution to the issues observed is to do something called Integrator Pre-Loading. You may have in the past 
used the PI block within Simulink and seen the following option to initialize the integrator values but may 
not have realised the use in this up until this exercise. 
Figure 6: Initialization Settings within the PI Block in Simulink
You should hopefully recognise that these integrator values reach a steady value when all the system 
derivatives equate to zero. Note these values down, and inside your integrator block, initialise the 
integrator to these values. 
In doing this, what has changed in the system? Can you explain with reference to the state-space system 
why the system now behaves the way it does? What assumptions are made performing this change, and 
can would anything need to be considered when apply on physical hardware?
Your system should be working incredibly well, in comparison to Exercise 3. However, there is still one issue 
that still needs to be resolved regarding the integrator. You probably will not see the issue if you have 
incorporated the integrator pre-loading successfully, so we need to do a step test to highlight this issue. 
The following would typically be quite代 写EEEE4116、MATLAB
代做程序编程语言 irregular for a grid tied inverter, but we are doing this to push the 
system into new behaviour. Use a ‘Step’ block on one of your references and apply a sizeable step input at 
half your simulation time (enough time to allow the system to stabilize before the step reference is 
applied). What can you observe happen with the system inputs, and why is this an issue? (If you don’t see 
any issues, apply larger steps, and analyse all states and inputs until a possible issue may arise)
We need to limit the effects of this issue, which raises the issue if integrator anti-windup. If a controller has 
an integrator, integrator anti-windup means that the integrator is turned off, when the when the system 
hits limit values (or “Saturates”). Can you explain why we would want to turn off the integrator when our 
system reaches these conditions?
In Figure 7 is the typical structure of an integrator anti-windup. Analyse the structure, adapt your system to 
incorporate the Anti-windup scheme below. Clearly describe your methodology and show you understand 
what is going on when you incorporate into your system.
Figure 7: Integrator Anti-windup Structure
If in your system for example, you want to limit u to ±60, the look-up table can be as follows:
Input -100 -60 -59.9 0 59.9 60 100
Output 0 0 1 1 1 0 0
Exercise 5 – Real Model Testing
The final part to this coursework is to apply our controller model to an actual circuit to show what has been 
designed would work. Here it is not expected that you build up a whole inverter simulation, and you will be 
provided with the backbone of the inverter model as shown in Fig 8.
To the simulation, you may choose two pieces of software, MATLAB Simulink, or PLECS. Please ensure you 
download the respective file for the application you wish to use. PLECS simulates power systems faster, but 
MATLAB is easier to implement the observer design through script. Both pieces of software will produce 
the same results for the same system.
Figure 8: Simulink Model of Three-Phase Inverter which can be downloaded on Moodle.
You task here is to adapt the model to incorporate the controller, into the switching model. One 
implemented, stress test your controller, note any differences between the average model you made for 
Exercises 1-4, and try and explain why such differences are observed. You may notice that Iiq is a non-zero 
value when the system reaches steady state. Can you explain why Iiq is the value to which it settles at?
Stress testing can involve load testing using the connected resistors and apply the switch at a given 
moment in time. Can change the reference values.
What happens to the system is you set the resistors to be a very low value (approx. 100Ω)? Can you explain 
why the converter behaves in this way?
Exercise 6 – Optimal Controllers Development
The methods of controller design covered in this coursework has been a very popular industrial method of 
controller design for many years, and you have in fact used similar techniques when designing PI controllers 
in your previous studies. However, as computers have become faster, and algorithms refined, techniques of 
controller optimisation have been rapidly employed in industrial applications. 
In this exercise, we will briefly look at the Linear Quadratic Regulator (LQR). The mathematics of the control 
is relatively complex and out of the scope for this module, however it is important to understand how you 
can use software tools such as MATLAB to develop optimal controllers for state-space systems. 
What is LQR Control? 
The Linear Quadratic Regulator is an Optimal Control method, which looks at how to drive a system from its
current states to the required reference states at a minimal cost, or in other words, to achieve our system 
references using the least amount of energy. 
To achieve this, the following cost function is evaluated:
min 𝐽 (𝑢) = ∫(𝑥
𝑇𝑄𝑥 + 𝑢
𝑇𝑅𝑢)
∞
0
Where the feedback control law, as normal is defined by Eq. 3:
𝑢 = −𝐾𝑥
Therefore, the algorithm looks for a value for a controller K, which not only makes the closed loop system 
stable, but minimizes the overall control effort J.
The way we can tune this system is by selection of the Q and R matrices. With reference to Eq. 2 you can 
see that the Q matrix weights the states ‘x’, and the R matrix weights the inputs ‘u’.
The Q and R matrices are each diagonal matrices, whose dimensions are equivalent to the number of 
states, and inputs respectively. For example, for our system we would have the following matrices, if Q and 
R are set to unity. 
𝑄
6×6 = [
1
⋱
1
],𝑅
2×2 = [
1
0
0
1
]
Each diagonal element in the Q matrix corresponds to the state of that row. For example, the non-zero 
value on the 2nd row of Q, associates a weighting to the 2nd states of x, in this case Iiq.
Likewise, for R, the non-zero value on the 1st row of R corresponds to the 1st input, in this case md.
Tuning the controller can be done by understanding these rules:
• Q Matrix Tuning
o The smaller the weighting in given states, the more controller effort provided to this state. In 
other words, the smaller the value, the faster the bandwidth for that given state.
• R Matrix Tuning
o The larger the weighting given to the inputs, the more restricted the respective input.
▪ So, a smaller value for R, the faster an input can react to system changes.
▪ A larger value for R, the more restricted, and thus slower an input would react. 
Eq. 2
Eq. 3
Eq. 4
For this exercise, you are going to develop an LQR controller, making use of the lqr() function within 
MATLAB. The control architecture will not need any change to that from Exercise 4  5. 
Read through the documentation of the lqr() function in MATLAB and ensure you know how to use the 
function. 
In your report, describe the process of designing the LQR controller, noting that we still desire our system to 
stabilise at our references within 200ms. Show a good design philosophy and describe the process in which 
you designed the controller to meet the specification. You may also want to think about the following 
questions in your report when writing about your design:
What performance improvement can you observe between the controller designed using LQR and pole placement in our closed loop system?
What are the fundamental differences between LQR and other control techniques such as pole-placement, 
and what advantages can use LQR optimisation, and which scenarios might it be a preferable method of 
design? 
What are the trade-offs involved when choosing the weightings in Q and R? Can you show the impact of 
changing each of the matrices and relate it back to theory?
Given the name of this controller is a Linear Quadratic Regulator, do you believe these controllers would 
work well in non-linear environment, and if not, are there some work arounds you could propose?
Writing your report
The style of the report should be a technical report. You may use the headings for each exercise to break 
down the subheadings in your report, but the style of the report you submit for your assignment should be 
in the style as if you were creating a professional document colleagues may look to understand your design.
Do not write your report as if answering an exam, i.e Ex5 a) Ex5 b). Doing this you will lose marks in 
presentation for your report.
Each exercise has a few questions which you are expected to answer, however if there are other elements 
of the design which you wish to discuss and analyse further based on your studies on the course or extra 
reading, you are highly encouraged to write up on this. This is how you will achieve the top marks in the 
rubric. 
Coursework Support Sessions
Each week there is a coursework support session on teams. This is your opportunity to ask any questions 
relating to the coursework, from use of MATLAB, Simulink, or general theory on controller design. You’re 
highly encouraged to attend each of the seminars each week and come prepared with questions.
The support sessions take the following structure each week:
• First 10-15 minutes will involve a small presentation or demonstration on different aspects of the 
controller design, or small software tutorial.
• Remaining 45 minutes will be a group QA. Please do have questions ready as this part of the 
session is directed by you. 
• If there is time remaining 1-to-1 help sessions can take place. It will be first come first serve. If I am 
unable to get to people requested, I will attempt to meet you another time in the week when I am 
free. 
For any coursework questions, please contact Dr. David Dewar (David.Dewar1@nottingham.ac.uk). 
Submission Requirements:
This report should be no longer than 20 pages (title pages and contents page are not included) 
Please do not include anything which might identify you as the writer of the report. All reports are to be 
marked anonymously. 
Therefore, please DO NOT INCLUDE any names, student ID’s or emails in the document. 

         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
