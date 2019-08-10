# survey
Database for Surveys

Details at  
"db design - survey/quiz with questions and answers"  
http://stackoverflow.com/q/540885/631619  
and  
http://stackoverflow.com/q/1764435/631619


https://stackoverflow.com/questions/540885/what-mysql-database-tables-and-relationships-would-support-a-qa-survey-with-con

Survey Database Design
Last Update: 5/3/2015
Diagram and SQL files now available at https://github.com/durrantm/survey

enter image description here :Survey Database Design

If you use this (top) answer or any element, please add feedback on improvements !!!

This is a real classic, done by thousands. They always seems 'fairly simple' to start with but to be good it's actually pretty complex. To do this in Rails I would use the model shown in the attached diagram. I'm sure it seems way over complicated for some, but once you've built a few of these, over the years, you realize that most of the design decisions are very classic patterns, best addressed by a dynamic flexible data structure at the outset.
More details below:

Table details for key tables
answers
The answers table is critical as it captures the actual responses by users. You'll notice that answers links to question_options, not questions. This is intentional.

input_types
input_types are the types of questions. Each question can only be of 1 type, e.g. all radio dials, all text field(s), etc. Use additional questions for when there are (say) 5 radio-dials and 1 check box for an "include?" option or some such combination. Label the two questions in the users view as one but internally have two questions, one for the radio-dials, one for the check box. The checkbox will have a group of 1 in this case.

option_groups
option_groups and option_choices let you build 'common' groups. One example, in a real estate application there might be the question 'How old is the property?'. The answers might be desired in the ranges: 1-5 6-10 10-25 25-100 100+

Then, for example, if there is a question about the adjoining property age, then the survey will want to 'reuse' the above ranges, so that same option_group and options get used.

units_of_measure
units_of_measure is as it sounds. Whether it's inches, cups, pixels, bricks or whatever, you can define it once here.

FYI: Although generic in nature, one can create an application on top of this, and this schema is well-suited to the Ruby On Rails framework with conventions such as "id" for the primary key for each table. Also the relationships are all simple one_to_many's with no many_to_many or has_many throughs needed. I would probably add has_many :throughs and/or :delegates though to get things like survey_name from an individual answer easily without.multiple.chaining.
