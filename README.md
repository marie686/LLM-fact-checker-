# LLM-fact-checker-
Rag that pulls relevant articles to fact check a claim, implemented with lang chain and wikipedia api
The prompts are based on the methods of human fact checkers which is broken down into three main steps where we look for three custom metrics: Credibility score, Bias score, and Evidence Strength.

     Corroboration: Do other sources agree with this statement?
  
    We use Serpapi to retrieve 10 articles related to the claim
         We ask GPT4 to classify a set of articles on the same topic as corroborating, contradicting or neutral. 
         We calculate Credibility score as 
        $$\frac{N_{\text{corroborating}}}{N_{\text{corroborating}}+N_{\text{contradicting}}}$$
         Rationale: Mirrors evidence agreement signals popular in human fact-checking 
 
     Bias-checking: Does this source have certain political leanings?
    
        Political leaning: GPT-4 assigns {Left, Right, Center, Mixed}. We map these to a Political Index: Left = −1, Right = +1, Center = 0, Mixed = 0.
         Tone: GPT-4 assigns {Emotional, Neutral}. We map to Tone Score: Emotional = 1, Neutral = 0 (binary sentiment proxy).
        Metric:  Bias=Political Index+Tone Score
    
     Evidence-based Reasoning: Is there strong evidence to support this claim?
  
        We ask GPT4 to classify supporting evidence as strong or weak. 
         Evidence Strength is calculated as:
         $$\frac{N_{\text{strong}}}{N_{\text{strong}}+N_{\text{weak}}}$$
   
     Final Trust Score: 
    
         The trust score is calculated as $$\text{Trust score}=w_1*\text{credibility score}+w_2*\text{1-bias score}+w_3*\text{evidence score}$$
        if above .5 we classify as true and weights are decided by the llm 
         Weights: In this milestone, w_1,w_2,w_3 are prompt-set and LLM-instantiated (reported in the JSON output)
     Predictions are evaluated with sklearn.metrics. Confusion matrices are reported per class.

    
