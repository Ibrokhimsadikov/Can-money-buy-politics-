import pandas as pd

# Example dataframe
data = {
    'survey_id': [1, 1, 1, 2, 2, 2],
    'survey_question': ['osat score', None, None, 'osat score', None, None],
    'survey_score': [5, None, None, 7, None, None]
}

df = pd.DataFrame(data)

# Step 1: Create a dictionary of osat scores by survey_id
osat_scores = df[df['survey_question'] == 'osat score'].set_index('survey_id')['survey_score'].to_dict()

# Step 2: Assign osat score to rows with missing survey_question or survey_score
df.loc[df['survey_question'].isnull() & df['survey_score'].isnull(), 'survey_question'] = 'osat score'
df.loc[df['survey_score'].isnull(), 'survey_score'] = df['survey_id'].map(osat_scores)

# Display updated dataframe
import ace_tools as tools; tools.display_dataframe_to_user(name="Updated Survey Data", dataframe=df)
