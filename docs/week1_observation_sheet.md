# Group Reflections (Group 4 - YouTube Recommendation System)

**Group Members:**
1. Amir Asyraaf Bin Zainal Abidin (A24AI0016)
2. Nazmi Bin Sidek (A24AI0069)
3. Amina Asyiffa Binti Aspiyah Mahyus (A23AI0015)
4. Siti Noor Balqis Binti Yuslimi (A24AI0090)

## Part A Reflection

**Question:** *After the group-sharing session, compare conversational AI systems with recommendation systems. Identify one quality dimension that differs between them and support your answer using an observation presented by another group.*

**Answer:**
Comparing our analysis of the YouTube Recommendation System with the findings presented by other groups on conversational AI systems (ChatGPT and Claude.ai), the Topic 1 quality dimension that differs the most between these two types of systems is **Accuracy (Factual Correctness) versus Personalisation Relevance and Adaptability**.

1. **How Conversational AI Systems Work (Observations from Other Groups):**
   In conversational AI systems like ChatGPT and Claude.ai, users ask direct text questions and expect a single accurate answer. Other groups observed that when they asked a factual question (*"What is the capital of Malaysia?"*), system quality was measured by **Accuracy** - whether the AI gave the factually correct answer ("Kuala Lumpur") without hallucinating. When they tested out-of-scope requests (*"Can you book a flight for me?"*) or harmful prompts (*asking for instructions to access someone else's account without permission*), the systems immediately responded with clear text explanations and safety refusals.

2. **How Recommendation Systems Differ (Observations from Our YouTube Test):**
   In contrast, a recommendation system like YouTube does not have one "factually correct" home page answer, and it never explains its decisions in text. Instead, its quality depends on balancing **Personalisation Relevance, Adaptability, and Fairness**:
   * **Personalisation vs. Adaptability:** Before our test, our home page (`Figure 1`) recommended our regular interests like TikTok remix music (*DJ Astaga Bercanda*), local entertainment (*Ikhmal Nour*, *Zizan Razak*, *Upin & Ipin*), and gaming content (*Mobile Legends: Bang Bang*) based on past watch history and subscriptions. While ChatGPT or Claude immediately switch topics on the very first prompt, when we searched for *"how to build a house underwater"* (`Figure 2`) and watched a video for 5 minutes, YouTube's main home page thumbnails did not switch to underwater building videos at all (`Figure 3`). It protected our long-term habits (still showing music mixes, *Manchester City vs PSG* football highlights, and podcasts) and only adapted slightly by adding a new **"Survival skills"** topic button on the top category bar (`Figure 3`).
   * **Fairness and Lack of Transparency:** Unlike conversational AI which communicates openly via text, YouTube operates silently at the Application layer with only basic labels like *"#ON TRENDING"* or *"Mix"* and zero explanation of why videos were picked. Comparing `Figure 1` and `Figure 3`, the exact same *DJ Astaga Bercanda* thumbnail stayed in the top-left spot, and almost every recommended video came from huge channels (*beIN SPORTS Asia* with 776K views and *Zizan Razak* with 18M views), giving zero visibility to small creators under 10k views and trapping users in a filter bubble with clickbait titles like *"POV Of A Penyondol"*. Because users cannot simply click "dislike" on their favorite music or gaming videos without ruining their normal feed, recommendation systems require user controls like our proposed **"Study Mode" toggle** on the homepage to temporarily prioritize educational videos.

---

## Part B Reflection

**Question:** *Identify which component or interaction has the most potential failure points. Explain what this suggests about where AI Engineering effort should be concentrated, referring to the quality dimensions introduced in Topic 1.*

**Answer:**

1. **Component with most annotations:**
    The Ranking Model and Recommended Home Feed interactions contain the highest concentration of "what could go wrong?" annotations. Some of the specific risks identified by the map include popularity bias in the Ranking Model, which makes it difficult for lesser-known people to get visibility and also Over-reliance on past behavior, where users fall into a filter bubble and end up seeing only similar content all the time. Moreover, there is no transparency in the Recommended Home Feed, hence "no explanation of why a video appears."

2. **Where engineering effort should be concentrated:**
    This means that the efforts of the engineers should be focused on the Ranking Model to counteract the problem of popularity bias, and to add diversity to recommendations, as well as to implementing the Content Policy Filter. Moreover, there should be created an explanation layer for the User Interface to ensure that users know why they receive particular pieces of content and how they can modify their recommendations.

3. **Connection to Topic 1 Quality Dimensions:**
    This directly ties into the information collected in Part A. The system displayed poor Adaptability since viewing a 5-minute underwater video did not change the primary feed, which means that it is rigid when it comes to prioritizing past history over the current intent of the user. This also indicates poor Fairness since it promotes huge channels, for example, a video with 18 million views despite warnings on the map about the bias toward popularity. Lastly, about Personalization Relevance, although the system does manage to identify the long-term interest of the user, it does not manage to cater to the present interest of the user. Since the Top 10 recommendations kept revolving around the same music and gaming videos, it stops the user from finding new, relevant content.
