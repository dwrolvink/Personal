# Terminology
In this section, I'll briefly explain the different terminology used to make talking about "goals" more granular and better defined.

#### Wishes and constraints
The process of making (and maintaining) a Gestalt planning starts with the stakeholders' **wishes** and **constraints**. 

When listing all the wishes, the stakeholder should only list two kinds of wishes: the **important** and the **nice-to-have**. 
Unrealistic wishes are to be excluded. 

An unrealistic wish is any wish that is clearly impossible given the constraints.
For example: having a big garden and a pool, when the constraints are a budget of 200.000 and the location is the center of 
Amsterdam.

When prioritizing, each wish gets assigned with a weight.
Constraints can be thought of as immutable wish: they have to be met exactly in any design.

> 'Assigning with weights' is a theoretical term. It is not advised to actually give a numerical value to each wish to be used in
a mathematical calculation. Humans are well enough equiped to do this process in their heads, perhaps with the support of a ranked
priority list.

#### Goals
Humans always wish for more than they can have. Even when those wishes are by themselves not unrealistic, combinations of those 
whishes will often exceed the boundaries that the constraints give us.

Aside from that, many wishes can be too vague to be actionable. One might list 'cosy' as a wish for their house, but if a third 
party were to take this into consideration when writing up a design, that party has to know what cosy means to the 
stakeholder. Thus, in the process of generating goals, any 'high level' wish has to be translated first into a 'low level' wish,
e.g. 'wooden ceiling beams in all rooms', or 'a fire place in the living room'. The more specific one writes down the goal here,
the easier the design process will become, whether this is done by a third party or the stakeholder itself.

When the goals are clear and specific, one more thing is needed for proper prioritization. This often has mainly to do with the
constraints of cost and time. As much information about the project has to be known in advance. One wish might be to break open a
wall between the living room and the kitchen, but what if there is a lot of plumbing going through that wall? This might rank up
the cost of realizing such a wish, and might move this wish down a few pegs to facilitate in realizing other wishes.
A stakeholder might also be clear on its own constraints, but in the dark about external constraints, such as municipal regulations.

Just like the wishes, the goals have to be low level (specific and clear) and ranked by priority. Where the wishes were divided into
important and nice to have, the goals generated in this process will be either on the backlog, or not on the backlog.

#### Change of plans
This might all sound an awful lot like Waterfall planning. The main differences will become apparent later on. The ideas of Agile,
Scrum, and Kanban are powerful, but their strength lies in very specific workloads. Many, if not most of all workloads will greatly
benefit from decent upfront planning. Whether this is done in a very strict (and not-Agile) way, or in a very informal, non-binding
way. This method will heavily lean into the latter. In fact, it is build entirely around the latter.

For a change of plans, one thing needs to happen: a change in the information available to us. This can be a **change of 
information**, or a **change of insight**. 

A change of insight means that no information (as collected in the planning phase) has surfaced or changed, but that the ideas
about that information has changed. This kind of change can often be prefaced with "on a second thought...".

A change of information might be the aforementioned discovery of plumbing going through a wall (this time during the progress
phase), or a change in the financial situation of the stakeholder.

#### The plan
The previous section was a bit of a detour. After defining the goals, these have to be first translated into 'plans'. 
A **declarative design** and an **imperative plan**. 

A declarative design might include an architectural design (in this example).
The declarative design can be called the **Ideal** for short: it describes in a very exact way the state you want your house to be
in when the project is finished. Think of materials, floor plans, wiring layouts, etc. 

It should be clear that there is no place for **any** wishes, or goals, or vagueness, in the declarative design. A low level 
goal might be "more than x dB of soundproofing between living spaces". In the design plan, you will only list the state of the
walls. This state is either defined to be compliant or not compliant to the soundproofing goal. If the soundproofing goal is
discovered to not be attainable during the design process this will be listed as a change of information, and this might result
in the stakeholder lowering the soundproofing goal (which could either be called a change of priorities, or a change of insight).
Every part of the design has to be compliant to the listed goals before the progress phase can commence.

Where the declarative design talked about the 'what', the imperative plan will talk about the 'how'. This includes the actual
planning of activities, as well as the methods used to arrive at the Ideal. One might for example state that certain activities 
can be done in paralel, while others have to be sequential. This might evoke the image of traditional project planning graphs, as
well it should. There's no use in 'scrumming out a finished kitchen' when the working crew still has to carry material and 
equipment through it to finish construction on the living room. 

> The explanation of the Imparative plan is a bit thin in this example, because in most cases, you'd let the contractor decide
the 'how', thus you are only left with listing all activities and orchestrating them in a required order. In the area of IT though,
the project planning and the technical know-how will often be located in the same deparment, if not the same team. In this case
one could be way more elaborate in defining the actual steps to reach the ideal.

During the design of the imperative plan (and during the progress phase for that matter) changes of information and insight might
occur at any moment. After the declarative design is completed though, any change might influence other decisions. A judgment 
has to be made that the changes to the declarative plan are local changes, or whether they affect other elements of the design
(i.e. the gestalt). 
One might think that changing the tiling material in the kitchen is a local change, but this might only be true if it doesn't 
affect the soundproofing, insulation, or any other properties of the house at large. Still, any effects felt would reasonably be
local in area to the kitchen. A better example is the choice of (not) breaking through a wall, as this will change many things in
the design, from the wiring/plumbing plans, to the way the house is lived in. Such a change would necessitate a complete 
declarative redesign.

> This is not to say the entire design has to be redone from scratch. One has to merely go through every element in the design,
and see whether it is affected by the change, and whether the change in goals still leaves every element in the design compliant
to said goals.


