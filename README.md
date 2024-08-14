# CDDO AI Exposure and Skills Importance Analysis
# **Introduction**
This analysis conducted by the Central Digital and Data Office uses a Large Language Model (LLM) to assess job descriptions. It evaluates the extent of AI exposure across different job roles and analyses the relationship between AI exposure and the required skill types. 

This analysis can be used to compare different types of job roles, and highlight both the types of roles and types of specific tasks that are highly exposed to AI. This should provide departments with evidence on which types of AI tools and interventions are likely to have the biggest impact across their workforce.


The analysis is divided into two main sections:

- **AI Exposure Analysis:** Uses an LLM to assess each individual task outlined in a job description and assign each task a score of between 0 and 1 to represent the level of exposure to AI.
- **Skills Analysis:** The O*NET database lists a total of 35 key skills that are used across different occupations. This analysis uses an LLM to assess both the full list of tasks and skills outlined in a job description and assign a score between 0 and 1 to each skill depending on its level of importance.

# **Prerequisites and Setup**

## Recreate the environment

It is recommended to create a new virtual environment with your preferred tool. The dependencies can then be installed with:
```sh
pip install -r requirements.txt
```

This analysis uses GPT Open API to analyse the content of job descriptions. To perform this analysis, users must have an OpenAI API Account ensuring that the account has sufficient credit to conduct the analysis. 

- **Create Account:** Create an account with OpenAI if you don't already have one. Sign up at OpenAI's website: https://platform.openai.com/signup

- **Generate API Key:**
  - Log in to your OpenAI account.
  - Navigate to the API Keys section: https://platform.openai.com/account/api-keys
  - Click on "Create new secret key" and save it securely. This key is necessary for the script to function.

- **Set API Key:** Insert your API key into the script where indicated in the section titled "Set API Key".

- **Check Account Balance:** Make sure that your OpenAI account has sufficient funds to cover the API usage costs. You can do this from the Billing page: https://platform.openai.com/account/billing in your OpenAI account. The total cost will depend on the length of task and skill information and total number job roles. As a guidline, the analysis would typically cost less than £10 for a sample of 150-200 roles, depending on input data length.

# **Data Required**
Each of the two sections of analysis requires different input data to complete:

### **AI Exposure Analysis:** 
The input data requires 3 columns. Data for each role should be over multiple rows as shown in the table below:
- **role:** Job title taken from the job description
- **task:** Each task outlined in the job description. Each task must be in a separate row so that the LLM can assess the level of AI exposure for each task individually
- **grade:** The grade of the role as outlined in the job description. These should be standardised to make analysis easier (EO, HEO, SEO, G7, G6)

**Example data input for AI Exposure Analysis:**

| **role**                | **task**                                                                                   | **grade** |
|-------------------------|--------------------------------------------------------------------------------------------|-----------|
| Executive Assistant      | Support the Senior Leadership Team to ensure their diary runs smoothly and their email inbox is in order so they use their time most effectively and with maximum impact     | EO        |
| Executive Assistant      | Arrange meetings, phone calls, and events with internal and external stakeholders          | EO        |
| Executive Assistant      | Manage core office functions and processes including the coordination of a daily information pack and weekly forward look note            | EO        |
| Customer Service Advisor | Speaking to customers on the phone, helping them with their questions or issues.           | EO        |
| Customer Service Advisor | Helping customers to pay the correct amount of tax at the right time.                      | EO        |
| Customer Service Advisor | Taking payments by phone and via our online services.                                      | EO        |

### **Skills Analysis:** 
The input data requires 2 columns. Data for each role should be in a single row:
- **role:** Job title taken from the job description. These should match the job titles used in the AI Exposure Analysis section.
- **skills:** A full list of all skills outlined in the job description. The full list of skills should be in a single cell.

**Example data input for Skills Analysis:**

| **role**                | **skills**                                                                                                                                                                              |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Executive Assistant      | Proactive people with strong interpersonal skills who enjoy working in a delivery focused agile environment; People with a strong attention to detail; Excellent written and verbal communication with the ability to communicate concisely and coherently to a wide variety of audiences; Experience of working with a wide range of stakeholders in a challenging and fast-paced environment; Demonstrable experience ability to deliver diplomacy and maintaining confidentiality; Ability to use Google’s G Suite office apps and an Apple MacBook is desirableacross multiple priorities at pace; Demonstrable experience of handling sensitive information displaying tact and and diplomacy and maintaining confidentiality; Ability to use Google’s G Suite office apps and an Apple MacBook is desirable      |                                                           |
| Customer Service Advisor | High standard of literacy and effective written communication skills; Effective verbal communication skills in particular the ability to provide clear advice over the telephone; Ability to deal with a demanding workload and prioritise accordingly; Ability to work on own initiative and as a member of a team; Effective negotiation skills; Ability to ensure that the highest standards of quality and customer care are achieved; Excellent interpersonal skills with people at all levels; Ability to use standard office IT packages; Ability to make presentations|

# **Data Analysis**
This analysis can be used in two main ways. Users can choose to use a full list of all job descriptions for their organisation to provide a comprehensive overview of AI exposure across the entire organisation. Alternatively, users may choose to use a sample of common job descriptions across all the organisation to highlight the types of role that are likely to be more exposed to AI. At least 20 different job roles should be used in the analysis. Including a much higher number of job roles will produce more robust analytical outputs. The key analytical outputs produced by the code for each section include:

### **AI Exposure Analysis**
This analysis will provide an exposure score for each task, and an average exposure score for each role. The exposure scores can be used to highlight the tasks and roles that are most exposed to AI. Specific analytical outputs in this section include:
- **Distribution of Exposure Scores across roles:** Distribution of average exposure score for each role categorising each role into very low exposure (<0.25); low exposure (between 0.25 and 0.5); medium exposure (between 0.5 and 0.75) and high exposure (>0.75).
- **Potential for Automation and Augmentation:** Categorising all roles depending on if AI is likely to automate or augment the tasks completed. Roles are categorised based on the mean and standard deviation of exposure scores given to tasks in each job role:
  - **Augmentation Potential:** mean < 0.4  and mean + std_dev  > 0.5 
  - **Automation Potential:** mean > 0.6 and mean - std_dev > 0.5

### **Skills Importance Analysis**
This analysis provides an importance score for 35 different skills for each role, and an average importance score for each role across 5 core skill categories. These scores highlight the types of skills required to complete different roles. The analysis in this section looks to highlight the relationship between AI exposure and types of skills required, with specific analytical outputs in this section including:
- **Regression of average skills importance vs exposure score:** Each of the 35 key skills is categorised into one of five skill categories calculating the average importance score for each category. This analysis conducts a regression to understand the relationship between each skill category has on exposure to AI using the following formula:
  - exposure_score = β0 ​+ β1⋅analytical_skills + β2​⋅managerial_skills + β3​⋅mechanical_skills + β4​⋅social_skills + β5​⋅fundamental_skills + β​6⋅grade + β7​⋅ddat
- **Average Importance Score across Job Grade:** Analysis showing the variation in importance score for each skill category.

# **Acknowledgments**
The methodologies used in each section of this analysis are adapted from publications and academic papers conducting similar research:
- **AI Exposure Analysis:** We apply the methodology and LLM prompts used in a recent paper published by the International Labour Organisation: 
  - Gmyrek P., Berg J., Bescond D. (2023). Generative AI and jobs: A global analysis of potential effects on job quantity and quality ILO Working Paper 96 (Geneva, ILO). https://doi.org/10.54394/FHEM8239
- **Skills Importance Analysis:** We adapt the methodology used in a 2023 paper published by the Pew Research:
  - Pew Research Center (July 2023) “Which U.S. Workers Are More Exposed to AI on Their Jobs?”

# **Contact**
This analysis was conducted by the Central Digital and Data Analysis Team. For further information or inquiries, please contact:

Andrew Ledingham - CDDO Lead Economist
(andrew.ledingham@digital.cabinet-office.gov.uk)


