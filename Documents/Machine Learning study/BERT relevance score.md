- #https://mccormickml.com/2020/03/10/question-answering-with-a-fine-tuned-BERT/
BERT first converts words to an id table, and using ids to convert to tokens.


the relevance score is what is computed when using GPT liked. 


```python
  

def score(q,a):

"""This function uses Fine-tuned BERT to calculate the relevance score between question and answer pair. It is trained on Standard Question Answering Dataset(SQuAD)"""

input_ids = tokenizer.encode(q, a) # adding sep and converting them to ids

# Apply the tokenizer to the input text, treating them as a text-pair.

# BERT only needs the token IDs, but for the purpose of inspecting the

# tokenizer's behavior, let's also get the token strings and display them.

tokens = tokenizer.convert_ids_to_tokens(input_ids)

  

# For each token and its id...

for token, id in zip(tokens, input_ids):

# If this is the [SEP] token, add some space around it to make it stand out.

if id == tokenizer.sep_token_id:

print('')

# Print the token string and its ID in two columns.

print('{:<12} {:>6,}'.format(token, id))

  

if id == tokenizer.sep_token_id:

print('')

  
  

tokens = tokenizer.convert_ids_to_tokens(input_ids)

# Search the input_ids for the first instance of the `[SEP]` token.

sep_index = input_ids.index(tokenizer.sep_token_id)

  

# The number of segment A tokens includes the [SEP] token istelf.

num_seg_a = sep_index + 1

  

# The remainder are segment B.

num_seg_b = len(input_ids) - num_seg_a

  

# Construct the list of 0s and 1s.

segment_ids = [0]*num_seg_a + [1]*num_seg_b

  

# There should be a segment_id for every input token.

assert len(segment_ids) == len(input_ids)

start_scores, end_scores = model(torch.tensor([input_ids]), # The tokens representing our input text.

token_type_ids=torch.tensor([segment_ids])) # The segment IDs to differentiate question from answer_text

```