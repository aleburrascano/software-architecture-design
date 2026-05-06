---
title: "Domain-Driven Design in Software Development: A Systematic Literature Review on Implementation, Challenges, and Effectiveness"
source: "https://arxiv.org/html/2310.01905v4"
author:
  - Ozan Özkan


Önder Babur


Mark van den Brand
published:
created: 2026-05-06
description: "Context: Domain-Driven Design (DDD) has gained significant attention in software development for its potential to address complex software challenges, particularly in the areas of system refactoring, reimplementation, and adoption. Using domain knowledge, DDD aims to solve complex business problems effectively. \nObjective: This SLR aims to provide an analysis of existing research on DDD in software development, paint a picture of DDD in solving software problems, identify the challenges encounte"
tags:
  - clippings
  - "vault-scout"
  - "source:tier2"
  - "kind:article"
  - "score:7"
---





Abstract
Context: Domain-Driven Design (DDD) has gained significant attention in software development for its potential to address complex software challenges, particularly in the areas of system refactoring, reimplementation, and adoption. Using domain knowledge, DDD aims to solve complex business problems effectively. 
Objective: This SLR aims to provide an analysis of existing research on DDD in software development, paint a picture of DDD in solving software problems, identify the challenges encountered during its application and explore the results of these studies. 
Method: We systematically selected 36 peer reviewed studies and conducted quantitative and qualitative analyzes to synthesize the findings. 
Results: DDD has effectively improved software systems, with its key concepts. The application of DDD in microservices has gained prominence for its ability to facilitate system decomposition. Some studies lacked empirical evaluations, highlighting challenges in onboarding and the need for expertise. 
Conclusion: Adopting DDD benefits software development, involving stakeholders such as engineers, architects, managers, and domain experts. More empirical evaluations and open discussions on challenges are needed. Collaboration between academia and industry advances the adoption and transfer of knowledge of DDD in projects.


keywords: 
Domain-Driven Design , DDD , Software Architecture , Software Development , Systematic Literature Review


††journal: Journal of Systems and Software
\UseRawInputEncoding


\affiliation[TUe]organization=Mathematics and Computer Science, Eindhoven University of Technology,city=Eindhoven,
country=The Netherlands
\affiliation[WUR]organization=Information Technology Group, Wageningen University and Research,city=Wageningen,
country=The Netherlands

{graphicalabstract}

{highlights}

This Systematic Literature Review (SLR) on Domain-Driven Design (DDD) highlights its potential benefits in software development and the importance of involving key stakeholders for successful implementation.
Studies suggest a growing interest and adoption of DDD in the context of microservices since 2017. However, more empirical research is needed to fully understand the benefits and challenges of implementing DDD in different software development scenarios.
The study reveals variations in the implementation and evaluation of the DDD principles in the included studies.
To improve the quality of DDD research, future studies should focus on the consistent use of DDD principles, robust evaluation methodologies, and open discussion of both advantages and limitations. This approach can provide a comprehensive understanding of DDD’s potential and practical implications
The implementation of DDD significantly depends on the expertise of stakeholders. Experienced developers and domain experts are crucial to effectively applying DDD concepts and practices.
Collaboration between academia and industry plays a crucial role in advancing the knowledge and adoption of DDD. Academic studies contribute to theoretical advances, while industry-focused studies showcase practical implementations and real-world applicability.


1 Introduction
In the field of software development, the pursuit of effective design methodologies and architectural approaches has been a crucial endeavor for both researchers and practitioners. Among these approaches, DDD has gained significant attention and adoption, particularly in the context of solving software architecture problems and addressing challenges related to refactoring, reimplementation and adoption [1]. DDD, as advocated by Eric Evans in his influential book, ”Domain-Driven Design: Tackling Complexity in the Heart of Software” [2], offers a framework for understanding, modeling, and solving complex business problems by placing domain knowledge at the core of the design process.
DDD emphasizes the alignment of software systems with the underlying business domain, enabling developers to better understand and model complex business requirements. By capturing the language, concepts, and relationships within the domain, DDD helps to identify and address software architecture challenges effectively [2, 3]. This approach provides guidance on how to improve system quality, maintainability, and scalability by ensuring that software design closely mirrors the business domain [2].
Although DDD presents solutions with its conceptual elegance and potential benefits in managing software architecture challenges [2, 3], it is crucial to evaluate its practical application and assess its impact on software development projects. This Systematic Literature Review (SLR) aims to provide a comprehensive analysis of existing research studies that have used DDD for various purposes in software development, such as improving system design, enhancing maintainability, and facilitating communication between stakeholders. By analyzing and synthesizing empirical studies, we aim to gain insight into the effectiveness of DDD in solving software architecture problems and identify the challenges encountered during the application of DDD from these studies.
To achieve this objective, a rigorous research methodology was employed that adheres to established guidelines proposed by Kitchenham et al. [4] and Wohlin et al. [5]. A group of 36 peer-reviewed studies was systematically selected and quantitative and qualitative analyzes were performed based on the data extracted from these studies.
The conclusion drawn from this SLR emphasizes the potential benefits of adopting DDD in software development. The study highlights the importance of involving key stakeholders, including software engineers, architects, project managers, and domain experts, to ensure a successful implementation. In addition, the study identifies the importance of open discussions on challenges and limitations and emphasizes the collaboration between academia and industry to advance the adoption of DDD and the transfer of knowledge in real-world projects.
This study contributes to the growing body of knowledge on DDD and provides valuable insights for practitioners and researchers seeking to apply the principles of DDD effectively in software development. By identifying the strengths and weaknesses of existing research and proposing areas for improvement, this SLR aims to foster the further advancement and adoption of DDD principles in contemporary software development projects.


1.1 Domain-Driven Design Essentials
DDD is a conceptual framework and approach to software development in software engineering that seeks harmony between the structure of software code and the complexities of the business domain it serves. The goal of DDD is to bridge the gap between technical implementation and business requirements, resulting in more effective communication between developers and domain experts [2, 3]. This concept was pioneered by Eric Evans, whose significant book Domain-Driven Design: Tackling Complexity in the Heart of Software [2] has shaped this paradigm. Evans emphasizes the alignment of software design with the intricate nuances of the business domain. He argues that effective software development requires a deep understanding of the complexities of the domain, allowing developers to create software that genuinely resonates with real-world needs [2].
In DDD, the domain refers to the specific area of expertise or the subject matter of the software application. This could be anything from e-Commerce, finance, healthcare, or logistics. The central concept in DDD is the development of a rich, well-defined domain model that accurately represents the business rules, processes, and entities within the domain. This model is built through collaboration between developers and domain experts, often using a shared language called Ubiquitous Language that captures both technical and domain-specific terms.
Through its concepts, which are explained in the following sections, DDD introduces principles that help software architects effectively represent the complex aspects of the real world. By embracing these core tenets, software engineers navigate the complex landscape of software development with a compass calibrated to the nuances of the domain, thus achieving a symbiotic fusion of technical precision and domain understanding.


1.1.1 Ubiquitous Language
At the core of DDD lies the concept of Ubiquitous Language, a practice that aims to bridge the gap between technical jargon and domain-specific vocabulary [2]. Ubiquitous Language fosters a shared vocabulary that harmonizes conversations among developers, domain experts, and business stakeholders. By encapsulating the complexities of the business domain in a common understanding, Ubiquitous Language eliminates ambiguity and streamlines communication. This shared linguistic framework empowers stakeholders to articulate domain intricacies effortlessly and translates directly into the software’s code, facilitating a direct and unambiguous translation of domain understanding into the software’s design and implementation.



1.1.2 Bounded Context
The Bounded Context concept effectively sets the boundaries of distinct zones where the semantics of a domain model are consistently applied. In applications of substantial scale that incorporate a variety of domain models, there is an inherent risk of ambiguity and misalignment. Bounded Contexts serve as strategic divisions, separating different domain models to maintain their integrity. This approach confines each domain to specific contexts, thus preventing semantic conflicts and ensuring a unified representation of the business domain within each delimitated area [2]. It can serve as an abstraction of a (sub-)system or team, aligning boundaries with software components, organizational structures, and responsibilities.




1.2 Context Mapping
In DDD, Context Mapping is a strategic practice used to understand and manage the relationships between different bounded contexts in complex software systems [2]. A Bounded Context defines the boundary within which a particular domain model is defined and applicable. However, in real-world systems, multiple teams often work on different subdomains or systems, each with its own Bounded Context. These contexts must interact and integrate, which introduces the risk of model contamination or ambiguity across teams.
Context Mapping provides a visual and conceptual technique to explicitly define how bounded contexts relate to each other and how integration should be handled. It helps development teams determine appropriate integration patterns (e.g., Shared Kernel, Customer/Supplier, Conformist, or Anti-Corruption Layer) depending on the nature of team collaboration, legacy constraints, or organizational structure. These relationships are represented using a Context Map, which becomes an essential communication artifact in large systems involving multiple teams and domains.
Evans emphasizes that strategic design decisions are essential when teams develop models in parallel, and that context mapping serves as a bridge to align models and avoid conflicts [2]. In practice, context maps also guide technical implementation, by informing decisions about API design, data ownership, and service integration. As such, context mapping is a vital part of ensuring semantic consistency and architectural integrity across distributed systems.


1.2.1 Aggregates
The concept of Aggregates encapsulates Entities, Value Objects, and Domain Logic through methods in entities and the use of domain services. Aggregates are the main points for accessing and changing data, ensuring data integrity within business transactions [2]. By containing data within defined boundaries, Aggregates optimize data management and system scalability while also enforcing logical boundaries that prevent uncontrolled data manipulation.



1.2.2 Entities and Value Objects
Entities and Value Objects differentiate within the DDD framework. Entities embody tangible elements within the domain, each possessing a unique identity. These entities change over time, capturing the mutable aspects of the business reality [2]. In contrast, Value Objects represent conceptual attributes that lack identity and remain immutable. By distinguishing between these two constructs, DDD provides a complete representation of the domain, accurately modeling tangible entities and abstract attributes [2].



1.2.3 Domain Services
Domain Services encapsulate domain logic that transcends individual entities and value objects [2]. These services orchestrate complex operations and interactions within the domain. By segregating multifaceted logic, domain services promote modular, reusable, and concise code. This approach mirrors the coherent coordination of real-world processes, enhancing the software’s alignment with the domain’s intricacies.



1.2.4 Domain Events
Domain Events is a concept used to model and communicate changes or significant occurrences within a domain [3]. They play a role in capturing and representing the behavior and state changes within an application’s core domain. A Domain Event is a lightweight object that represents something important that has happened in the domain. These events are usually named in the past tense to indicate that they’ve already occurred. They help decouple different parts of a system by allowing different components or aggregates to communicate without needing to know the specifics of each other. This promotes a more modular and maintainable architecture.



1.2.5 Anti-Corruption Layer (ACL)
The concept of an ACL protects the domain model from external influences. This layer mediates interactions between different models or systems, preserving the domain’s integrity. By translating data between disparate representations, the ACL ensures a consistent domain model, safeguarding it from external distortions [2].



1.2.6 Core Domain
Core Domain is a central concept of strategic design in DDD, which refers to the part of the software system that delivers the most value and differentiates the business. The goal is to focus development effort on the Core Domain to gain competitive advantage, often assigning the most skilled developers to it [2].



1.2.7 Shared Kernel
Shared Kernel refers to a pattern in which two Bounded Contexts share a common subset of the domain model. This shared part must be carefully managed to avoid conflicts and preserve the integrity of the model, often requiring close collaboration and coordination between teams [2].
The ACL shares similarities with several well-known design patterns. For instance, the Remote Proxy pattern [6] provides a local representative for an object that resides in a different address space, mediating interactions between local and remote systems. The Wrapper pattern [7] encapsulates an object to provide a different interface, much like the ACL encapsulates external systems to present a consistent interface to the domain model. The Adapter pattern [6] converts the interface of a class into another interface that a client expects, similar to how the ACL adapts external data representations to match the internal domain model.
The fundamental contribution of DDD lies in creating software systems that closely mirror the business’s cognitive schema. By converging domain experts, business stakeholders, and software developers, DDD facilitates synergistic collaboration to deliver software solutions that optimally cater to business needs. This collaborative effort fosters the combination of domain insights, augments the comprehension of problems and solutions across the entirety of the development team, and supports the fostering of inter-team relationships.



1.2.8 Strategic and Tactical DDD
Over time, the DDD community has categorized DDD patterns into two broad groups: Strategic and Tactical. Evans also has a similar cateogization in his book [2] as he calls it Strategic Design, but the community calls it as Tactical Design as of today. It has become a widely adopted framing, especially in practitioner literature such as Vernon’s work [3]. Strategic DDD focuses on aligning the software model with the broader business strategy and organization, with key concepts including Bounded Context, Core Domain, Context Maps, and Anti-Corruption Layer. In contrast, Tactical DDD includes concrete modeling techniques used within a Bounded Context, such as Entities, Value Objects, Aggregates, Domain Services, and Domain Events. Throughout this study, we use this distinction as a lens to interpret the usage patterns of DDD concepts in the literature.



1.2.9 Event Storming
In addition to modeling patterns, DDD also includes collaborative modeling techniques that help bridge the gap between business and technical stakeholders. One widely adopted practice is Event Storming, a workshop-based approach introduced by Alberto Brandolini [8], where domain experts and developers collaboratively explore business processes by identifying and organizing domain events, commands, aggregates, and actors. This method is particularly effective for discovering Bounded Contexts and aligning the domain model with real-world business operations.



1.2.10 Other DDD Patterns and Concepts
The DDD patterns and concepts introduced in this section reflect those discussed most frequently in the 36 primary studies reviewed. Although there are additional patterns in the DDD literature, such as Customer/Supplier, Open Host Service, Conformist, and Published Language, these were not consistently represented in the reviewed studies and, therefore, were not included in detail. Our goal in this section is to provide a focused overview of the key patterns that emerged from the literature rather than an exhaustive catalog of all DDD concepts.





2 Related Work
To the best of our knowledge, despite the growing interest in DDD as a software development approach, there is a scarcity of SLRs specifically focused on DDD. While the literature landscape on DDD is not completely unexplored, previous studies have primarily touched upon certain aspects remotely related to DDD.
For instance, Le et al. [9] propose a set of concrete design patterns for Domain Modeling within the context of Object-Oriented DDD, providing valuable insights into the practical application of DDD in specific modeling scenarios. Singjai et al. [10], on the other hand, examine the interrelation between microservice APIs and DDD, shedding light on how DDD principles can be applied in the context of microservices architecture. However, it is worth noting that some related systematic literature reviews have been conducted in adjacent domains. Goey et al. [11], for instance, undertakes a Systematic Literature Review on Design-Driven Innovation, exploring design-driven approaches and methodologies in innovation processes, which, although related to design-driven practices, do not directly address the specific principles and patterns of DDD. Similarly, Bertoni et al. [12] conducts an SLR on Data-Driven Design in Concept Development, focusing on the role of data-driven approaches in the early stages of concept development but not explicitly addressing the nuances of DDD.
Despite valuable contributions from these studies, none of them provide a comprehensive review of DDD as a whole, addressing its principles, methodologies, and practical applications. While these prior studies offer valuable perspectives related to design-driven or microservice-oriented software development, they do not conduct a structured or comprehensive SLR focused on the conceptual and practical aspects of DDD itself. In contrast, our study takes a DDD-centric perspective, systematically analyzing how DDD principles, patterns, and stakeholders are discussed and applied in peer-reviewed literature. This focus enables us to capture insights specifically tied to DDD adoption, effectiveness, and evaluation, filling a gap not directly addressed by adjacent literature.



3 Research Objectives and Method
This study employed a Systematic Literature Review methodology to create generalizable empirical results (as opposed to specific and in-depth yes less generalizable research methods such as case studies [13]) to (1) consolidate the existing knowledge on the utilization of DDD in the domain of software development, (2) identify gaps in the current research, guiding future studies and helping to focus on underexplored areas, and (3) for practitioners, a comprehensive review can offer insights into best practices, common challenges, and effective strategies for implementing DDD in real-world projects. The research approach adhered to established guidelines as proposed by Kitchenham et al. [14] and Wohlin et al. [5].
Additionally, this study delves into further detail regarding the Goal and Research Questions (Section 3.1), Search Strategy and Data Sources (Section 3.2), Exclusion Criteria (Section 3.3), Primary Study Identification and Selection (Section 3.4), and Data Extraction (Section 3.5) in subsequent sections. By examining these aspects in a meticulous manner, this study aims to provide a comprehensive and rigorous analysis of the research topic at hand.


3.1 Goal and Research Questions
The primary objective of this research is to investigate and synthesize the current state of research on the application of DDD in software development, to understand its benefits, challenges, used patterns, and effectiveness. To achieve this objective, the following research questions (RQ)s have been formulated:




RQ1: What is the state of the art concerning DDD in the existing literature, in terms of study types, research venues, software systems, and the distribution between academic and industrial settings?



RQ2: What categories of software systems have demonstrated benefits from the implementation of DDD?



RQ3: Which specific software development concerns have been addressed through the application of DDD in previous studies?



RQ4: What are the most common DDD patterns utilized in previous research, such as ACLs, Bounded Contexts, and other related approaches?



RQ5: What are the challenges encountered during the implementation of DDD in software development projects?



RQ6: How has the effectiveness of DDD been assessed and measured in prior studies?



RQ7: Who are the key stakeholders involved and benefited in the implementation of DDD?






3.2 Search Strategy and Data Sources
In line with established protocols and our previous studies [15, 16], we meticulously designed our search strategy to identify pertinent studies from reputable academic databases renowned for their extensive coverage of computer science and software engineering literature. The databases utilized in this study were ACM, SpringerLink, IEEE, and Wiley, all of which are widely recognized for their authoritative and diverse repositories of articles.
The search query employed in this investigation was thoughtfully crafted to encompass a broad spectrum of articles relevant to our research objectives. The query utilized a combination of keywords and logical operators to target studies explicitly discussing DDD in conjunction with software development activities. We used ”Domain-Driven Design” because we explicitly wanted to capture studies that employed DDD. When using only ”Domain-Driven Design”, we encountered a significant number of false positives, such as studies from medicine. Therefore, to ensure the relevance of the captured studies, we included implementation keywords to specifically address studies that employed DDD in practice. The search query was formulated as follows:

"Domain Driven Design" AND (refactor* OR improv* OR optimiz*
OR restructur* OR migrat* OR evolution OR cleanup OR
reengineering)


We strategically used asterisks (*) as wildcards to capture variations of the specified keywords and to ensure inclusivity in the search results.



3.3 Exclusion Criteria
The establishment of selection criteria was undertaken with the aim of mitigating bias and minimizing subjectivity within the study. The inclusion and exclusion criteria were formulated in accordance with the research questions (RQs) as delineated by Kuhrmann et al. [17], thereby ensuring that the criteria align closely with the intended objectives of the investigation. Following our criteria, (1) we meticulously identified and removed duplicate studies that appeared in more than one of the selected databases to prevent the inclusion of redundant information. To maintain the comprehensiveness of our analysis, (2) we excluded studies that lacked full-text availability, as the absence of complete content may hinder a thorough evaluation of the study’s findings and methodology. Given the language limitations of our review team, (3) we restricted our analysis to studies written in English to ensure accurate and consistent interpretation and analysis. In the interest of focusing on primary research articles, (4) we excluded books, book chapters, editorials, and issue introductions from our analysis, as they may not present original empirical studies or detailed investigations. (5) We excluded studies that were not primary research studies (e.g., review articles, meta-analyses, opinion pieces) to maintain a clear focus on empirical research directly relevant to our research questions. To ensure the inclusion of studies closely related to DDD and its practical applications, (6) we excluded studies that did not explicitly present or discuss the introduction of a method, technique, tool, or evaluation pertaining to DDD or its adaptation and extension. The list of the exclusion criteria can be seen in Table 1.

Figure 1: Study selection progressed from a database search to the quality assessment. The final set of primary studies advanced to the data extraction stage. The count of studies in the evaluation steps is displayed in parentheses.



Table 1: List of exclusion and inclusion criteria.




3.4 Primary Study Identification and Selection


3.4.1 Conducting Manual Database Search
In the first stage, we performed a meticulous and systematic manual search of the databases mentioned in Section 2.2. The search was carried out in March 2023 and yielded a total of 707 studies, which were 153 studies from ACM, 19 studies from IEEE Xplore, 510 studies from SpringerLink, and 25 studies from Wiley, as also shown in Table 4. To ensure the exclusion of duplicate results, we conducted conflict resolution, resulting in the identification of one duplicate paper. After eliminating duplicates, we advanced 706 unique studies to the next stage of the selection process.



3.4.2 Applying Exclusion Criteria
In the second stage, we applied our predefined exclusion criteria to the 706 studies. The screening process involved the assessment of each paper based on its title, abstract, and keywords. In situations where a thorough evaluation was not possible based on these elements, we briefly reviewed the introduction and conclusion of the paper, following the guidelines suggested by Wohlin et al. [5]. To ensure a rigorous assessment, we adopted a multi-assessor strategy, where 2 authors independently evaluated the studies without knowing each other’s assessments. Due to the high number of studies, we performed the assessment in iterations until we achieved at least 0.80 Cohen’s Kappa coefficient [18], which indicates almost perfect agreement between the assessors [18]. Once the desired Cohen’s Kappa was reached, the first author continued to assess the remaining studies to determine their inclusion or exclusion, following the approach done in previous studies [19, 20].
In each iteration, we randomly selected 30 studies for assessment. After each iteration, we conducted conflict resolution sessions to discuss the rationale of our assessments. We eventually achieved 1.0 Cohen’s Kappa coefficient in the second iteration. The remaining studies were evaluated by the first author, resulting in 31 studies marked for inclusion and progression to the next stage. The distribution of these included studies across the search databases is presented in Table 4.
Table 2 provides a detailed breakdown of the number of studies excluded under each exclusion criterion. This complements the overall metrics shown in Table 4, providing transparency on how rigorously the exclusion process was applied.


Table 2: Number of studies excluded per criterion during the screening process.




3.4.3 Conducting Forward Snowballing
In the third stage, we conducted forward snowballing to make our study population wider and ensure relevancy. We conducted the forward snowballing in two approaches. We conducted a forward snowballing process for studies that cited Evans’ book [2] since Evans holds significant prominence in the field of DDD due to his influential contributions. His book, which serves as a reference, has gained substantial recognition and is widely regarded as a crucial resource among practitioners in the software development community [1, 3, 21, 22]. To conduct this snowballing, we utilized Scopus as a database because of its centralized indexing of our target databases mentioned in Section 3.2 and to eliminate results from grey literature. The screening process for Evans’ book forward snowballing followed the same strategy as that used for forward snowballing in the primary studies. In the process of analysis, a comparison was made among the database search results, forward snowballing results, and the previously identified studies to identify any potential duplicates. As a result, a total of 10 duplicate studies were identified, and these duplicates were subsequently resolved in favor of the database search results and forward snowballing outcomes. Consequently, an additional 15 studies that did not meet any of the exclusion criteria were identified and subsequently included in our study. The comprehensive forward snowballing process, encompassing both the forward snowballing of included studies and the examination of studies cited in Evans’ book, resulted in a total of 23 studies being incorporated into our analysis.
We also performed forward snowballing following Wohlin et al. guidelines [5], we used Google Scholar to identify studies that cited our included primary studies. We assessed these additional studies based on their title, abstract, and keywords, using the exclusion criteria established earlier. In cases where a definitive resolution was not achievable based on the provided information, we conducted a rapid analysis of the entire paper, as suggested by Wohlin et al. [5]. Through this process, we identified 8 studies that did not meet any of the exclusion criteria and were therefore included in our study.



3.4.4 Quality Assessment
During stage 4 of our primary study identification and selection process, we proceeded with the task of conducting a comprehensive quality assessment of the selected studies. Adhering to the established guidelines put forth by Kitchenham et al. [4], each study was meticulously evaluated, with ratings assigned on a scale ranging from 0 to 1. On this scale, a rating of 1 denoted a positive affirmation, 0.5 indicated a partial fulfillment of the quality criteria, and 0 represented a negative response. The specific quality assessment questions utilized in this evaluation can be found in Table 3. Studies that achieved a total score of 4 or higher were deemed to have successfully met the quality assessment criteria and were subsequently included in the data extraction stage. In order to maintain the rigor and objectivity of the process, we adopted a multi-assessor strategy during the quality assessment stage. Each study was independently evaluated by two assessors. In cases of disagreement or doubt, the assessors consulted with each other to reach a consensus. This approach ensured that the quality assessments were rigorous, minimized potential biases, and allowed discrepancies to be resolved systematically. After quality assessment, we identified the 36 primary studies. The final list of primary studies can be found in Table 6.
Among the 36 selected studies, the majority demonstrated strong adherence to methodological rigor: 78% received a full score (1.0) for clearly stating research aims (QA1), and 75% scored full on explaining their methodology (QA2). However, some common weaknesses were observed. For instance, 39% of studies scored partially (0.5) or zero on reporting negative findings (QA5), indicating a tendency to report only positive outcomes. Additionally, 36% had incomplete descriptions of their data collection or analysis procedures (QA3), which limits reproducibility. These gaps reflect typical challenges in empirical DDD research, where reporting standards may vary, and certain studies lean more toward industrial experience than academic evaluation. Despite this, all selected studies met a minimum quality threshold, ensuring a consistent baseline for analysis.


Table 3: List of quality criteria adopted from Kitchenham et al. guidelines [4]. Each criterion is rated on a scale of 0 to 1. Studies that achieved a score of >=>=> =4.0 advanced to the data extraction stage.



Table 4: Primary study identification metrics showing accessed databases, search dates, number of resulting studies, conflicted studies, included studies, and the count of studies that passed quality checks.





3.5 Data Extraction and Synthesis


3.5.1 Data Extraction
In order to ensure consistency and alignment with our research questions and goals, a data extraction form was developed (Table 5). The initial list of data points was prepared by the first author, and this list was then discussed among the authors to refine and finalize a starting set of categories. A pilot data extraction was done on a subset of randomly selected primary studies. During the data extraction process, we adopted an iterative and incremental approach, continuously refining the categories as new insights emerged. This iterative refinement allowed us to adapt the data extraction form to better capture the nuances and specific details relevant to our RQs and goals.
The data points were categorized into 3 main categories. Firstly, the Publication Metadata category involved extracting basic information such as the title, authors, and publisher of each study. Secondly, the Study Characteristics category included data related to the study design, data collection methods, and evaluation metrics employed. Lastly, the Domain-Driven Design category focused on extracting information specifically pertaining to the application of DDD in each study.
Following the completion of the quality assessment stage, the final set of studies deemed relevant and has sufficient quality to be moved to the data extraction stage. The data extraction process was carried out by the first author using a predefined data extraction form. This form served as a structured tool to systematically collect and record pertinent information from the selected studies.


Table 5: Data Extraction Form.




3.5.2 Synthesis
In this study, the data extraction process involved the collection of both qualitative and quantitative data from the final set of primary studies to derive our results. Specifically, for research questions (RQs) encompassing 1 to 4, we focused on extracting quantitative data. The frequencies and percentages of each identified category were then computed and reported to address these specific research questions. This approach allowed us to quantify and present the distribution and prevalence of different factors and concepts relevant to the research questions under consideration.
Conversely, for RQs pertaining to RQs 5 to 7, we concentrated on extracting qualitative data from the primary studies. To synthesize and interpret these qualitative findings, we applied a narrative synthesis method based on the guidance provided by Popay et al. [23]. This involved systematically extracting key information and important findings from each individual study. Subsequently, we conducted a comprehensive analysis and interpretation of the extracted qualitative data to draw overarching and meaningful conclusions in response to the respective research questions. The narrative synthesis approach allowed us to integrate diverse qualitative evidence, explore patterns and themes across the studies, and provide a cohesive and comprehensive understanding of the research questions in question.


Table 6: List of primary studies and their quality assessment scores.






4 Results
This section presents a comprehensive analysis of the findings obtained from our SLR conducted on DDD and aims to address the research questions by providing a detailed exploration of the current state of research pertaining to DDD in the literature. Through a meticulous examination of various articles, studies, and sources, the section sheds light on the key insights and trends related to the implementation, effectiveness, and assessment of DDD in diverse software systems. By organizing the results around the research questions, this section offers valuable insights into the involvement of key stakeholders, the evaluation and measurement of DDD’s effectiveness, and the overarching research landscape concerning this domain-centric architectural approach.


4.1 (RQ1) What is the state of the art concerning DDD in the existing literature, in terms of study types, research venues, software systems, and the distribution between academic and industrial settings?
Based on our primary studies (36), the current state of research concerning DDD in the existing literature showcases studies conducted between 2006 and 2023. Among these studies, 23 (64%) are Conference Papers, 12 (33%) are Journal Articles, and 1 (3%) study is from the Preprint database, which also can be seen in Figure 2.

Figure 2: Breakdown of study types (left) and study designs (right).

In terms of venues, a diverse range of conferences and journals have emerged as platforms for the dissemination of DDD research. These venues span the spectrum of software engineering and technology domains. Among them, the Conference on Object-Oriented Programming Systems, Languages, and Applications takes a prominent role with 3 publications, showcasing its relevance in fostering discussions around object-oriented paradigms in software development. Additionally, the Symposium on Information and Communication Technology have contributed 2 publications.
The most common study type is Case Study, accounting for 10 (28%) studies, followed by Theoretical Study with 9 (25%) studies. There are also 4 (11%) Action Research studies, 3 (8%) Experience Reports, 2 (6%) Empirical Studies, 2 (6%) that are not explicitly mentioning a type but using empirical methodologies, Controlled Experiments, 2 (6%) Comparative Analyses, 1 (3%) Observational Study, 1 (3%) Methodological Study, and 1 (3%) Experimental Study. The remaining studies were primarily conceptual, methodological, or design proposals with limited or no empirical validation.
In terms of measurement rigor, 17 studies did not report any concrete evaluation metrics. Those studies are theoretical or design-oriented. Many propose methods, tools, or architectural patterns without validating them through empirical means. While some are labeled as case studies or experience reports, their evidence is often anecdotal and lacks systematic measurement. This pattern suggests a gap in empirically grounded research in certain areas of DDD, particularly tool development and architectural modeling.
When cross-referencing these findings with the application domains, we observed that empirical evaluations were more likely to be found in industrial case studies involving microservices and distributed systems, while studies focused on tools or frameworks tended to lack rigorous empirical assessment. Furthermore, studies based on action research or industry collaborations generally provided more detailed insight into DDD’s impact on software quality, maintainability, and organizational communication.
Regarding the software systems studied, Microservices received the highest attention with 16 (44%) studies, followed by Enterprise Software Systems with 6 (17%) studies, and DDD-related Tools and System Agnostic studies with 4 (11%) studies each. Additionally, Standard Computer Applications and Web Applications were subjects of 2 (6%) studies each, while Service-Oriented Applications and Distributed Systems each had 1 (3%) study.

Figure 3: Breakdown of the primary studies based on their field of focus (left) and field of focus by publication year (right).

Out of the 16 Microservices studies, 10 (63%) were conducted in an academic environment, while the remaining 6 (38%) were performed in industrial settings. In contrast, Enterprise Software Systems were predominantly studied in industry settings. Interestingly, DDD-related Tools were exclusively implemented in academic settings.
The interest in DDD initially emerged in industry settings, as evidenced by the first 3 studies conducted between 2006 and 2007. Subsequently, the focus shifted to academic settings, with all 3 studies from 2009 to 2012 being conducted in academia. The trend then became more balanced, with an equal distribution of studies between academic and industrial settings in 2016.

Figure 4: Breakdown of the primary studies based on applied software systems (left) and studies grouped by the study type and software systems (right).

The distribution of studies across academic and industry settings reveals an interesting pattern. Out of the total 36 studies, 21 (58.3%) were conducted in academia, indicating a dominant presence of academic research in this field. Conversely, 13 (36.1%) studies were conducted in industry settings, and 2 (5.6%) studies had a hybrid setting involving both academic and industry collaboration. This disparity in the academic-industry ratio suggests that DDD has garnered significant attention from the academic community, where researchers are actively exploring its concepts and applications. However, it also highlights the relevance and practical implications of DDD in industry settings, as evidenced by the notable number of industry-based studies.
The existing literature demonstrates a diverse and evolving landscape of research on DDD, with a particular emphasis on Microservices and an increasing number of academic contributions. This suggests a growing recognition of the relevance and applicability of DDD in both academic and industrial contexts.



4.2 (RQ2) What categories of software systems have demonstrated benefits from the implementation of DDD?
With this RQ, we present the categories of software systems that have demonstrated benefits from the implementation of DDD, based on the findings. The studies were analyzed, and their contributions to various software system categories were identified during the data extraction phase. The breakdown of the following data can also be visually seen in Figure 4.


4.2.1 Microservices
Several studies have highlighted the advantages of applying DDD principles in the context of microservices architecture [24, 27, 32, 35, 38, 39, 40, 46, 47, 48, 49, 50, 53, 54, 55, 58]. According to our learnings from those studies, by adopting DDD, organizations have been able to improve the modularity, maintainability, and scalability of microservices-based applications, enabling effective handling of complex and distributed systems. The use of DDD patterns and concepts in the design and development of microservices has been particularly beneficial in achieving clear boundaries between services, enabling better communication between development teams, and ensuring a strong alignment with business domains. For instance, Krause et al. [40] applied DDD principles to modernize a real-world lottery system into a microservices-based architecture, demonstrating the approach’s relevance in refactoring legacy systems in the public sector.



4.2.2 Enterprise Software Systems
Several studies have reported the benefits of applying DDD in the context of enterprise software systems [25, 26, 31, 37, 42, 52]. The adoption of DDD principles has proven valuable in handling the increasing complexity of enterprise systems and addressing challenges related to legacy system modernization. By emphasizing bounded contexts and domain models, DDD enables more maintainable and flexible enterprise software architectures, allowing organizations to better adapt to changing business requirements.



4.2.3 Web Applications
Some studies have shown that DDD can be beneficial in the development of web applications [30, 59]. By applying DDD principles, web application developers have been able to improve the design and implementation of Single Page Applications (SPAs), achieving better cross-framework compatibility and enhancing productivity in SPA development.



4.2.4 Tools and Standard Computer Applications
Several studies have applied DDD in the context of tools [28, 34, 56] and standard computer applications [29, 44]. DDD has been leveraged to improve code quality, enhance the software construction process, and provide more comprehensive solutions for domain modeling and software generation. In the case of tools, DDD has been employed to bridge the gap between business domain concerns and infrastructure, ensuring the maintainability and readability of code.



4.2.5 Distributed Systems
In this study, we classified systems based on the architectural types explicitly mentioned in the primary studies. However, some system types, such as Service-Oriented Architecture (SOA) (Section 4.2.6) and Distributed Systems, may overlap in practice. For example, SOA is one possible way of realizing a distributed system.
In cases where the primary study did not specify the exact architectural type such as microservices or SOA, but specifies it is a distributed system, we used the more generic term Distributed Systems to accurately reflect the nature of the system without over-specification.
This classification ensures that we do not make assumptions about the specific system architecture when such information is not available, and it provides a comprehensive view of how DDD is applied in various types of distributed systems. We acknowledge that system types like SOA and Distributed Systems are not always mutually exclusive, and we carefully categorized the systems according to the information available in the primary studies.
This approach was applied to Braun et al.’s study [36], where it reported that DDD has demonstrated benefits in the development of distributed systems. By controlling concurrent data access and achieving semantic compatibility in domain models, DDD has been instrumental in tackling consistency-related design challenges in distributed data-intensive systems. Similarly, Wang et al. [32] utilized DDD to architect a blockchain-based traceability system in the global supply chain domain, where managing bounded contexts across distributed actors was critical.



4.2.6 Service-Oriented Applications (SOA)
Landre et al. [33] use DDD patterns to improve the software architecture of a large enterprise system, which is designed as a SOA. The authors mention that establishing a DDD context map helped in scoping the project and identifying architectural problems such as complex dependencies and gateway roles.
In this study, we distinguish between SOA and Microservices as separate architectural classifications, not to make a strict ontological separation, but to reflect the terminology used in the primary studies themselves. While both paradigms promote modular and service-based decomposition, they also differ in typical implementations: microservices emphasize lightweight, independently deployable components with decentralized governance, whereas SOA is often realized with centralized orchestration and heavier protocols (e.g., ESB and SOAP) [60, 61].
We acknowledge that this boundary is not universally agreed upon, and many researchers recognize significant overlap between SOA and microservices. As noted by Lewis and Fowler [62], the distinction lies more in implementation philosophy than fundamental principles. Our categorization aims to reflect how the systems were described and studied in the reviewed literature, without implying that SOA and microservices are categorically distinct. Future work could explore how these architectural paradigms co-evolve in practice.



4.2.7 System Agnostic
Certain studies have shown that DDD can provide valuable contributions for the implementation of software which is suitable across various system categories [41, 43, 45, 51]. In these studies, DDD principles and patterns have been applied to implement practitioner frameworks and methods to address communication gaps between business specialists and software designers, improve software system modularity, and support the creation of context maps.



4.2.8 DDD Tools
DDD has been applied to improve code quality, readability, and maintainability through the development of supportive tooling. These tools fall into two main categories. Annotation-based DDD frameworks, such as Daileon by Perillo et al. [28], aim to separate business domain concerns from infrastructure code. In contrast, DDD modeling tools, such as Context Mapper by Kapferer et al. [34], provide higher-level support for visualizing and evolving Context Maps and other domain artifacts.




4.3 (RQ3) Which specific software development concerns have been addressed through the application of DDD in previous studies?
The SLR conducted in this study sought to identify the specific software development concerns that have been addressed through the application of DDD in previous research. The results revealed several challenges and motivations encountered in various software development projects, encompassing diverse domains such as microservices architecture, legacy system modernization, distributed systems, and complex decision support systems. The subsequent information present a comprehensive overview of the identified challenges and motivations for adopting DDD.


4.3.1 Addressing API Design Problems in Distributed Systems
One of the prominent challenges addressed by DDD in software development projects is related to API design in distributed systems, particularly in microservice-based systems. This challenge revolves around missing guidance on how to derive APIs and API endpoints from domain model elements. The primary concern is that coupling smells lead to poor API design in distributed systems. These issues result in shallow APIs that are difficult to understand, maintain, and evolve. Singjai et al. [24] propose clear guidance on how to derive APIs
and API endpoints from domain model elements using DDD patterns.



4.3.2 Investigating Performance in Component-Based Software
Command Query Responsibility Segregation (CQRS) and Event Sourcing (ES) are two architectural patterns often used in conjunction with DDD to improve scalability and traceability. CQRS separates write operations (commands) from read operations (queries), enabling systems to optimize each independently. Event Sourcing, on the other hand, persists all changes to the application state as a sequence of immutable events rather than storing the current state directly. In this approach, the current state is reconstructed by replaying these events. These patterns align well with DDD’s tactical building blocks—such as aggregates and domain events—and have been leveraged in studies to evaluate performance characteristics under different workload patterns [25].
In pursuit of achieving optimized performance and resource metrics in component-based software, DDD has been used in research to investigate the effect of workload patterns. Specifically, the research aimed to gain insight into how patterns like CQRS and ES are created, thus providing valuable guidelines for architects to select appropriate architectures based on workload characteristics [26].



4.3.3 Reducing Complexity in Currency Exchange Systems
A prevalent software development problem addressed by DDD pertains to reducing complexity and high coupling in currency exchange systems. The case in question involves the integration layer between a new currency exchange system (SPOT) and a legacy back office system (TBS), wherein the integration layer (TBSExport) had become an unwieldy burden with unpredictable side effects following minor changes. To tackle this complexity and streamline the integration process, researchers sought to devise effective solutions using DDD principles [26].



4.3.4 Microservice Size, Partitioning and Defining API Endpoints
Microservice architecture introduces the challenge of determining the appropriate size of individual microservices, thereby necessitating a conceptual methodology for partitioning microservices based on domain engineering techniques. Improperly sized microservices can lead to complexity management issues, where too large services become monolithic and too small services create an overly complex system. This can also result in performance bottlenecks and scalability challenges, as well as data consistency problems due to poorly coordinated related data spread across services. Additionally, undefined or poorly defined boundaries complicate maintenance and evolution, and misaligned microservice boundaries with team boundaries hinder team autonomy and require excessive coordination. DDD has been instrumental in addressing this challenge, aiming to achieve optimal microservice design and composition within the context of a microservices architecture [24, 27].



4.3.5 Improving Code Quality with Domain Annotations
In the pursuit of enhancing code quality, readability, and maintainability while ensuring a clear separation between the business domain and infrastructure concerns, DDD has been leveraged to address challenges associated with using annotations in DDD. The proposed concept of ”domain annotations” by Perillo et al. [28] serves to mitigate these challenges and fosters a robust software design approach.



4.3.6 Enhancing Software Construction and Development Environment
The lack of generative, module-based software construction and seamless development environment integration poses significant challenges in software development. Without these capabilities, developers face difficulties in automating the generation of software from domain models, leading to increased manual effort and potential for errors [29]. This can result in inconsistencies between the domain model and the implemented software, hindering maintainability and scalability. Additionally, the absence of integrated development tools means that developers must switch between different environments and tools, disrupting their workflow and reducing productivity. By addressing these issues, in the work of Le et al. [29] the adoption of the jDomainApp framework and an Eclipse IDE plugin has been proposed as a solution to these challenges, thereby optimizing software construction and development processes.



4.3.7 Multi-Platform SPA Development
The challenges faced by Single Page Application (SPA) developers encompass two crucial aspects: designing SPAs that can effectively work across different SPA frameworks (e.g., Angular, React, React Native, and Vue.js) and translating this design into an intermediate high-level language that can seamlessly transform into a target framework of choice. DDD offers a multi-platform, hierarchical approach to tackle these challenges, significantly enhancing SPA development productivity [30].



4.3.8 Scaling Agility in Large Organizations
Large organizations encounter a myriad of challenges when striving to scale agility, including effective coordination and communication between agile teams, inter-team dependencies, and the need for clearly defined requirements. By adopting DDD, enterprises have sought to address these challenges and provide architectural guidance to support large-scale agile development. However, it is important to acknowledge that successful DDD implementation in large organizations necessitates experienced enterprise architects to provide effective guidance [31].



4.3.9 Complexities in Blockchain-Based Traceability Systems (BTS)
BTSs are software systems designed to ensure transparent, tamper-proof tracking of products or data throughout a supply chain or process. These systems leverage blockchain technology to record transactions and state changes in an immutable ledger, enhancing trust, provenance, and accountability.
In the context of DDD, BTSs benefit from patterns such as Bounded Contexts and Aggregates to model complex interactions and responsibilities across different stakeholders and subsystems. As reported in [32], the application of DDD in BTS improves the maintainability and extensibility of these systems by clearly delineating domain boundaries and aligning technical implementations with business processes.
The design and development of BTSs pose unique challenges and complexities, including domain model intricacies, modeling boundaries, and managing the relationship between different bounded contexts. Researchers have employed DDD and microservice architecture to address these challenges, thereby improving the cohesiveness, maintainability, and extensibility of BTSs in handling complex data in the global supply chain in [32].



4.3.10 Challenges in Model Decomposition
Decomposing software systems into cohesive and loosely coupled modules poses significant challenges for software engineers and service designers. DDD patterns have been instrumental in addressing this challenge, offering a clear and concise interpretation of these patterns, despite debates and differing interpretations among practitioners [43].



4.3.11 Legacy System Modernization and Refactoring
Legacy systems often suffer from outdated technologies, poor documentation, and a lack of alignment between the system’s current state and the evolving business requirements. This misalignment creates a communication gap between business specialists, who understand the domain requirements, and software designers, who are tasked with implementing these requirements [48]. As a result, changes and enhancements become risky, time-consuming, and error-prone. DDD addresses these issues by providing a structured approach to align the domain model with business needs, facilitating better communication and understanding. This alignment enables more effective refactoring and modernization efforts, ensuring that the updated system meets current business requirements while maintaining or improving system integrity and performance [48, 51].


Table 7: Reported benefits of DDD across different software system categories, as identified in the primary studies.





4.4 (RQ4) What are the most common DDD patterns utilized in previous research, such as ACLs, Bounded Contexts, and other related approaches?
In this SLR on DDD, we investigated the most commonly utilized DDD patterns across various software systems. The analysis was based on a dataset comprising studies identified through our study selection process explained in Section 3.4. Each study was examined for the presence or absence of specific DDD patterns, marked by ”Y” for used and ”N” for not used patterns, during our data extraction process. Our results are also visually shown in Figure 5.
The results reveal that certain DDD patterns are prevalent in the majority of the analyzed studies, indicating their significance and widespread adoption in the DDD context. The following DDD patterns were found to be most commonly utilized:



1.
Ubiquitous Language. Ubiquitous language is a fundamental DDD pattern, and our findings indicate that it is frequently employed in various software systems, including Microservices, Enterprise Software Systems and Web Applications [25, 26, 27, 30, 31, 40, 42, 43, 48, 49, 50, 55].


2.
Bounded Context. Bounded contexts were found to be widely utilized in Microservices, Enterprise Software Systems and Web Applications [25, 26, 27, 30, 31, 35, 36, 38, 39, 41, 43, 48, 49].


3.
Entities and Value Objects. Entities and value objects, representing domain concepts, were extensively adopted in Microservices, Enterprise Software Systems and Web Applications [25, 26, 27, 30, 31, 32, 36, 41, 42, 43, 44, 45, 46, 48, 49, 50, 51].




Figure 5: Breakdown of the applied DDD patterns in the primary studies. Applied DDD patterns marked with ”Y” and not applied pattern marked with ”N”.

In contrast, the ACL was identified as the least used DDD pattern across different software systems. Several studies did not utilize the ACL in their respective projects [26, 29, 33, 34, 52, 56, 59].
The ACL serves as a means to protect the integrity of the main domain from external and legacy systems, and its limited adoption suggests that certain software projects may have addressed integration challenges through alternative approaches or may not have encountered a specific need for this pattern.
While Ubiquitous Language, Bounded Context, and Entities/Value Objects were widely observed across studies, patterns such as Core Domain and Shared Kernel were rarely reported. Only a limited number of studies (e.g., [33, 48]) referred to the Core Domain explicitly, often in the context of prioritizing critical business logic. Shared Kernel was not discussed as a standalone concept in any study, although collaboration between teams was sometimes implied in studies using Bounded Contexts. Similarly, upstream–downstream relationships—central to context mapping in Strategic Design—were briefly mentioned in a few cases but not analyzed in depth.



4.5 (RQ5) What are the challenges encountered during the implementation of DDD in software development projects?
The challenges encountered during the implementation of DDD in software development projects are diverse and demand careful consideration to ensure successful adoption. In this RQ, we present the key challenges identified through primary studies. To support the narrative discussion, we also summarize the main DDD implementation challenges and mitigation strategies identified across the primary studies. Table 8 presents a concise mapping that highlights both the barriers encountered and the approaches taken to address them in practice.


4.5.1 Addressing Design Complexities
Several studies highlight the complexity associated with implementing DDD patterns, particularly in the context of microservices and distributed systems. The integration layer between different Bounded Contexts can become burdensome, leading to high coupling and unpredictability in the system. This challenge, for instance, is exemplified in the case of the currency exchange system (SPOT) and the legacy back-office system (TBS) in Peng et al. [26]. Finding the right balance between high cohesion within services and loose coupling between them is crucial for scalability and maintainability [26, 27].


Table 8: Challenges in implementing DDD and corresponding mitigation strategies and techniques reported in the literature.




4.5.2 Identifying Optimal Microservices Boundaries
Decomposing monolithic systems into microservices presents its own set of challenges. Determining the appropriate size and boundaries of microservices is not straightforward and requires careful consideration of the domain and workload characteristics [27]. The granularity of microservices can be subjective, leading to potential disagreements among development teams [43]. Ensuring that microservices align with business needs and subdomains can be challenging [46]. Additionally, finding suitable examples that demonstrate DDD-driven microservices architecture is often noted as a limitation [54].



4.5.3 Managing Domain Model Complexity
When adopting DDD, managing the complexity of the domain model becomes a crucial challenge. Modeling boundaries between different Bounded Contexts and ensuring the relationships between them are properly managed require deep knowledge about the organization’s domain [45]. The complexity of the domain model and the interactions between bounded contexts can hinder the process of abstraction and generalization of elements [45]. This challenge is especially pronounced when dealing with large-scale and complex data-intensive systems [32].



4.5.4 Integrating DDD with Existing Frameworks
Integrating DDD into existing software frameworks can present compatibility issues, as DDD introduces encapsulation and abstraction levels that may not align seamlessly with existing structures [48]. Refactoring in an actively developed codebase can also prove challenging, especially when engineers lack prior experience with DDD [48].



4.5.5 Communication Gap between Business Specialists and Software Designers
Incorporating DDD requires effective communication between business specialists and software designers to ensure consistency in requirements and conceptual models [51]. Misalignment in communication can lead to discrepancies in the final design and implementation.



4.5.6 Model-Code Gap in Domain-Specific Language (DSL)
A domain-specific language (DSL) is a computer language specialized to a particular application domain [2]. Studies that focus on DSL-based DDD implementations point out potential weaknesses, including the existence of a ”model-code” gap [34]. The lack of a deeper knowledge about the domain may hinder the process of abstraction and generalization, leading to artifacts that may not align with the organization’s domain [45].



4.5.7 Impact of Developer’s Experience and Challenges in Onboarding
The level of experience and familiarity with DDD concepts among the development team can influence the effectiveness of DDD implementation [46]. Developers with prior experience in DDD are more likely to understand and apply DDD patterns effectively, resulting in better-designed and more maintainable software systems [46]. On the other hand, inexperienced developers may struggle with grasping the complexities of DDD and may not fully utilize the benefits of the approach [46]. Also, introducing DDD to a development team that has little or no prior exposure to DDD can be a challenge [48]. Onboarding developers and providing the necessary training and resources to understand DDD concepts and principles can require additional time and effort [48]. This challenge can be particularly pronounced in organizations that are transitioning from traditional software development approaches to DDD.



4.5.8 Navigating Controversial Debates
Controversial debates and differing interpretations exist among practitioners regarding the application and combination of DDD patterns [43, 48]. Developers may face challenges in deciding which patterns to adopt and how to effectively integrate them into the development process. This can result in disagreements within the team and may require additional effort to reach a consensus.



4.5.9 Need for Experienced Domain Experts
Incorporating DDD requires a deep understanding of the domain and business requirements. Experienced domain experts are valuable in guiding the development team to create accurate and effective domain models [54]. However, the availability of such domain experts may be limited, especially in specialized domains, and this can hinder the DDD adoption process.




4.6 (RQ6) How has the effectiveness of DDD been assessed and measured in prior studies?
The effectiveness of DDD has been assessed and measured in prior studies through various evaluation methods and metrics. It is evident from the data that prior studies have employed a combination of quantitative and qualitative evaluation methods to assess the effectiveness of DDD. These methods are described in more detail in the following subsections.


4.6.1 Performance and Resource Metrics
In the case of Maddodi et al. [25] the effectiveness of DDD was measured by evaluating the performance and resource metrics of the software system using Command Query Responsibility Segregation (CQRS) and Event Sourcing (ES) frameworks. The study reported relative error values for throughput and response time for different scenarios.



4.6.2 Feedback from Stakeholders
In [31] the effectiveness of DDD practices, such as event storming workshops and domain modeling, was measured through the adoption and use of these practices by agile teams. The study also considered feedback from stakeholders, including Program Managers, Product Owners, and Domain Architects, on the benefits and value of DDD in supporting large-scale agile software development.



4.6.3 Context Mapping and Complexity Reduction
Landre et al. [33] evaluated the effectiveness of DDD, particularly the strategic part of it, by using context mapping and context relationships to analyze the software architecture and identify complexities. The study demonstrated how DDD provides mechanisms to address the identified problems and improve the quality of the Enterprise Architecture and derived software architectures by applying responsibility layers and improving scoping of projects.



4.6.4 Effectiveness of Design Guidelines
Braun et al. [36] conducted an assessment to determine the efficacy of design guidelines pertaining to distributed data-intensive systems. The evaluation encompassed various metrics, including the proportion of compatible domain operations and the extent of trivial aggregates within the domain model.



4.6.5 Questionnaires and Developer Perception
The study by Ozkan et al. [48], the efficacy of DDD was evaluated using a combination of qualitative methods, including focus group sessions, semi-structured interviews, and think-aloud protocols. Additionally, a quantitative analysis was conducted using the Technology Acceptance Model (mTAM) questionnaire. The findings revealed that DDD positively impacted software maintainability, as developers perceived it. Similarly, Braun et al. [36] undertook an assessment of design guidelines effectiveness by gauging developers’ perceptions in their study.



4.6.6 Measuring Effectiveness of ECD3 Guidelines
In Braun et al. [38] the effectiveness of the DDD measured based on Eventually Consistent Domain-Driven Design Guidelines (ECD3), assessed by conducting a comparative analysis between compatible and incompatible domain operations in the redesigned domain models. The implementation of the recommended ECD3 guidelines exhibited a notable enhancement in the proportion of compatible operations within the domain models.



4.6.7 Software Generation and Complexity Analysis
The effectiveness of the DDD-based software generation method proposed in [29] was evaluated by examining its linear time complexity and scalability on real-world software of considerable magnitude, through the utilization of the jDomainApp framework.



4.6.8 Time and Resource Consumption
In [56] the effectiveness of including business domain concepts in the DSL was measured by evaluating the perceived quality, creation time, and length of test case specifications.



4.6.9 Granularity and Coupling Analysis
The study [54] assessed the effectiveness of DDD in microservices by comparing the coupling and cohesion values of different examples. The study concluded that original examples achieved a good balance between low coupling and acceptable cohesion.



4.6.10 Prototype Application
In [40] the effectiveness of the approach for modernizing legacy lottery applications into microservices was evaluated through its successful application to a real-world lottery application.



4.6.11 Metamodel and Design Models
Le et al. [30] aimed to assess the efficacy of DDD in constructing Single Page Applications (SPAs) compatible with multiple platforms. For this purpose, they developed a novel metamodel and implemented a hierarchical approach within the DDD framework. The outcomes of the study revealed that employing the domain model as a central component and adhering to the hierarchical DDD approach yielded favorable design models alongside prosperous generation of SPAs tailored for various platforms.




4.7 (RQ7) Who are the key stakeholders involved and benefited in the implementation of DDD?
In the implementation of DDD, several key stakeholders are involved to ensure its successful application. These stakeholders encompass a diverse set of roles, including software engineers, architects, project managers, and domain experts. Through our SLR, we have identified the following key stakeholders.


4.7.1 Software Engineers
Software Engineers play a central role in implementing DDD principles and practices in various software systems. They are responsible for translating the domain model into code, implementing domain-specific logic, and ensuring the alignment of the software architecture with the domain requirements [24, 25, 26, 41, 48].



4.7.2 Architects
Architects are crucial in the implementation of DDD as they provide strategic guidance and oversight on the overall software design. They collaborate with Software Engineers and other stakeholders to define the high-level architecture, bounded contexts, and domain models that capture the domain knowledge effectively [25, 43, 45].



4.7.3 Project Managers
Project managers play a vital role in coordinating DDD implementation efforts. They ensure that DDD practices align with project goals, allocate resources appropriately, and facilitate effective communication between different teams and stakeholders [26, 31].



4.7.4 Domain Experts
Domain experts are individuals who possess in-depth knowledge of the specific business domain for which the software system is being developed. Their active involvement is critical for refining the domain model, identifying key domain concepts, and validating the software’s alignment with real-world domain requirements [24, 31, 45].
In addition to the primary stakeholders mentioned above, the literature also identifies other relevant participants involved in the implementation of DDD. These participants include Quality Assurance (QA) Engineers, DevOps Engineers, UX Designers, Business Analysts, Systems Analysts, Data Modeling Specialists, and end-users [26, 38, 48, 55, 56, 59].
Taking our findings in consideration, we identified that the successful implementation of DDD requires close collaboration and communication among these diverse stakeholders to ensure that the developed software system effectively represents the domain’s complexity and meets the business needs. The active engagement of software engineers, architects, project managers, and domain experts, along with the contributions from other relevant participants, enables the application of DDD principles to create robust and domain-centric software solutions.

Figure 6: DDD studies grouped by their software systems and publication years.






5 Discussions
In this section, we aim to consolidate and evaluate the extensive range of information obtained through our RQs in order to present a comprehensive overview of the effectiveness, challenges, and potential consequences associated with the adoption of DDD in software development. Throughout this section, we will do an examination of the findings by establishing connections across various studies and identifying recurring themes and patterns. It is important to acknowledge that certain aspects of the discussion have already been addressed in Section 4 when answering to the RQs. Nevertheless, within this dedicated section, we will explore further nuances, ramifications, and potential applications attributable to DDD based on the available evidence.


5.1 Adoption and Implementation of DDD
Several studies report successful implementations of DDD, showcasing its ability to improve software design, maintainability, scalability, and agility. DDD concepts, such as Ubiquitous Language, Bounded Contexts, Aggregates, Entities, Value Objects, and Domain Events, are commonly utilized to model complex domains effectively. Many researchers emphasize the significance of defining clear domain boundaries and using a common language between domain experts and software designers to ensure effective communication and understanding. Additionally, DDD is frequently integrated with other software development paradigms, such as microservices and event-driven architecture, to achieve better modularity and maintainable systems.



5.2 Ubiquitous Language as a Core Principle of DDD
The data analyzed points out that the Ubiquitous Language stands as a foundational principle of DDD. It ensures a shared and consistent understanding of the domain between stakeholders, including business specialists, software designers, and developers [2]. The studies consistently emphasize the importance of Ubiquitous Language in facilitating communication, clarifying domain concepts, and improving collaboration among team members [24, 25, 27, 28, 29, 30, 31, 42, 46, 47, 48, 49, 51, 57].


Table 9: SWOT analysis of DDD in software research.




5.3 Bounded Context and Decomposition of Microservices
Bounded Context is another fundamental concept in DDD, and its significance in microservices architecture is evident in several studies [25, 26, 27, 31, 36, 40, 49, 50, 54, 59]. The concept of Bounded Context is used to define explicit boundaries where specific domain models apply, allowing the system to be decomposed into manageable, cohesive units. Decomposing a system into microservices based on Bounded Contexts enables easier scaling, maintenance, and independent updating of each microservice.



5.4 Domain Services, Entities, and Value Objects as a Tactical DDD
Several studies discuss the use of Domain Services, Entities, and Value Objects as part of DDD’s tactical patterns [25, 30, 31, 46, 48, 49]. Domain Services are employed to handle application tasks and technical code that is not part of the core business logic, while Entities and Value Objects represent different entities and their relationships in the domain model. These concepts contribute to modeling complex business domains and enabling object-oriented representations of domain concepts.



5.5 ACL for Maintaining Domain Integrity
When dealing with different bounded contexts, interacting with external systems or refactoring legacy systems, the risk of corrupting a domain model with concepts and structures from another context is high. The ACL acts as a safeguard, ensuring that each context’s domain remains untainted and independent. The concept of the ACL is discussed in some studies [26, 27, 33, 57]. It is utilized in the studies to manage dependencies between different domains, preventing the contamination of contexts, safeguard the domain model of a Bounded Context from changes in other contexts, segregating application-specific elements from domain-specific components in microservices and maintaining consistency between aggregates. Studies also suggests that ACL shields new contexts from potential contaminations or disruptions originating in legacy contexts.



5.6 All Hands on Deck: It is not Only About Software Engineers
Based on the data extracted from the literature, the implementation of DDD involves the active participation of various key stakeholders who play critical roles in ensuring its successful application. These stakeholders encompass a diverse set of roles, including Sofware Engineers, Architects, Project Managers, and Domain Experts. Software Engineers hold a central position in the DDD implementation process, responsible for translating the domain model into executable code, implementing domain-specific logic, and aligning the software architecture with domain requirements [24, 25, 26, 41, 48]. Architects contribute strategic guidance and oversight to the overall software design, collaborating with Software Engineers and other stakeholders to define high-level architecture, bounded contexts, and domain models that effectively capture domain knowledge [25, 43, 45].
Project Managers play a vital role in coordinating DDD implementation efforts, ensuring alignment with project goals, resource allocation, and effective communication between teams and stakeholders [26, 31]. Domain Experts, possessing in-depth knowledge of the specific business domain, actively contribute to refining the domain model, identifying key domain concepts, and validating the software’s alignment with real-world domain requirements [24, 31, 45].
Moreover, the literature also identifies other relevant participants involved in the DDD implementation, including QA Engineers, DevOps Engineers, UX Designers, Business Analysts, Systems Analysts, Data Modeling Specialists, and end-users [26, 38, 48, 55, 56, 59]. The successful implementation of DDD requires close collaboration and communication among these diverse stakeholders to ensure that the developed software system effectively represents the domain’s complexity and meets the business needs. The active engagement of software engineers, architects, project managers, and domain experts, along with the contributions from other relevant participants, enables the application of DDD principles to create robust and domain-centric software solutions. The involvement of these stakeholders enhances the alignment of software systems with real-world domain requirements and fosters the development of high-quality and effective software solutions.
Our review adds a nuanced view of how this collaboration evolves in practice. Rather than treating the developer–domain expert relationship as static, the primary studies reflect varied degrees of maturity in this interaction. For instance, some report structured practices such as Event Storming workshops [31], while others highlight onboarding challenges and communication gaps [48, 51]. This range of experiences suggests that bridging the gap is not a one-time activity, but an ongoing and often iterative process influenced by tooling, team experience, and organizational context — a perspective less emphasized in prior discussions.



5.7 Evaluation of DDD Effectiveness
The evaluation of DDD effectiveness varies across studies. Some studies present empirical evaluations based on metrics, user feedback, or controlled experiments [24, 25, 31, 36, 42, 48, 53]. However, several studies lack specific evaluation results or empirical evidence [28, 40, 47, 51, 58, 59].
To establish a more comprehensive understanding of the benefits and potential challenges of implementing DDD in different software development scenarios, further empirical research is needed. Researchers and practitioners should focus on collecting concrete metrics, conducting real-world case studies, and measuring the perception of practitioners. We believe that it is also essential to involve human metrics, such as user feedback and assessments from domain experts, architects, and experienced developers. Considering the challenges in implementing DDD, human-centric evaluations can provide valuable insights into the effectiveness of DDD and its impact on the software development process.



5.8 Rise of the Microservices in DDD
Our SLR reveals a significant shift in the popularity of microservices in DDD studies starting 2017 and at peak in 2021. As depicted in Figure 6, prior to 2017, credible DDD studies specifically focusing on microservices were scarce, indicating a lack of attention in this area. However, from 2017 onwards, we observed a notable increase in the number of studies exploring the application of DDD principles in the context of microservices. This trend aligns with the findings of a Systematic Mapping Study on Microservices conducted by Hamzehloui et al. [63] in 2018, which also reports a rising number of evaluation papers, suggesting that microservices have been gaining wider implementation in real-world software development scenarios.
Furthermore, Viggiato et al. [64] in 2018 highlights the increasing popularity of microservices architectures, further emphasizing the growing relevance and adoption of microservices in the software industry. In a similar vein, Knoche et al. [65] in 2019 notes that many companies are actively considering whether microservices are a viable option for their applications, indicating the considerable interest and attention surrounding the adoption of microservices-based architectures.
The convergence of our findings with those of the previously mentioned studies and experts further corroborates the growing significance of microservices in the domain of software development, especially in the context of DDD methodologies. The upward trend in microservices adoption is evident from the year 2017 onwards, indicating a turning point in the landscape of software development practices. Beyond identifying the rise in microservices, our study offers novel insights into how DDD contributes to shaping modularity in such architectures. In particular, we observe that practitioners increasingly rely on DDD patterns like Bounded Contexts and Aggregates to define service boundaries and promote semantic clarity. While prior studies may suggest that microservices benefit from domain-aligned decomposition, our findings ground this assumption in a broader empirical base across 36 peer-reviewed studies. This synthesis provides stronger evidence for how DDD actively influences microservice design practices in both academic and industrial settings.



5.9 Challenges and Considerations in Implementing DDD Effectively
To ensure the successful implementation of DDD, it is essential to employ DDD patterns accurately and efficiently. Proper utilization of DDD principles demands a skilled team consisting of experienced developers, domain experts, and seasoned architects [48]. The significance of experience in DDD cannot be overstated, as we have observed from various studies that onboarding to DDD is not a straightforward process, and the expertise of developers significantly influences the success of the implementation [43, 48].
Implementing DDD effectively requires a substantial amount of attention and effort, particularly when integrating it into existing software systems. The transition from traditional software development approaches to DDD can be challenging, and the process demands careful planning, training, and a clear understanding of the DDD concepts and principles. Seasoned developers with prior experience in DDD are more likely to grasp the complexities and intricacies of the approach, resulting in better-designed and more maintainable software systems [46]. On the other hand, inexperienced developers may struggle to fully utilize the benefits of DDD, leading to potential issues in the implementation process.
Furthermore, the involvement of domain experts plays a crucial role in the success of DDD implementation. Their deep understanding of the business domain is invaluable in guiding the development team to create accurate and effective domain models [54]. However, acquiring experienced domain experts can be challenging, especially in specialized domains. Their scarcity may pose hurdles in the adoption of DDD, but their involvement is crucial for crafting domain models that truly reflect the intricacies and requirements of the real-world domain.



5.10 Academia-Industry Collaboration
The academic studies in our SLR predominantly emphasize the theoretical aspects of DDD. They explore the foundational principles and patterns of DDD, such as Ubiquitous Language, Bounded Context, Entities, and Value Objects. These studies often propose novel frameworks, tools and methodologies that extend the DDD concepts to specific problem domains. While they provide valuable insights into the theoretical underpinnings of DDD, they might lack empirical validation and practical implementation in real-world projects.
On the other hand, we experienced that the industry-focused studies put DDD principles into practice in various software systems. They showcase how DDD is applied to real-world projects and explore its impact on system design, architecture, and development. These studies often involve case studies and experience reports from software engineers, architects, and domain experts. The emphasis is on practical implementation, challenges faced, and lessons learned while applying DDD in complex software projects.
Our findings indicate a healthy mix of academic and industry studies, reflecting the growing interest in DDD from both research and practical application perspectives. While academic studies contribute theoretical advancements and innovative approaches, industry-focused studies demonstrate the real-world applicability and effectiveness of DDD principles in solving complex software design and development challenges. More interaction between academia and industry in DDD research could potentially facilitate the transfer of knowledge and best practices, contributing to the further advancement and adoption of DDD principles in real-world software development scenarios, as successfully demonstrated in [48].


5.10.1 Contrasting Academic and Industry Perspectives
While our study includes a balanced set of academic (58.3%) and industry (36.1%) studies—with a small number of hybrid cases (5.6%)—the practical perspectives from industry deserve further distinction. Academic research on DDD tends to focus on conceptual modeling, tool development (e.g., DSLs, context mapping tools), and theoretical discussions on patterns such as Bounded Contexts or Entities. These studies often propose frameworks or methodologies but may lack concrete empirical validation. In contrast, industry studies are generally more experience-driven, describing real-world applications of DDD in large-scale systems such as microservices, enterprise refactoring, or system modernization. These studies prioritize architectural pragmatism, performance concerns, and integration challenges, and often highlight the social or organizational implications of DDD adoption (e.g., communication, onboarding, and team alignment).
For example, the academic studies often examine how to formalize context boundaries using modeling tools like Context Mapper or domain-specific languages, whereas the industry studies report practical experience applying bounded contexts in cross-functional teams or resolving communication issues between developers and domain experts. Moreover, industry papers tend to use anecdotal or case-based evidence, while academic studies more frequently advocate for abstract, generalizable models. This contrast reveals complementary strengths: academia offers methodological structure and design guidance, while industry grounds these concepts in complex, evolving development environments. Recognizing this distinction helps to contextualize the relevance and maturity of DDD research across settings.




5.11 A Critical Analysis of Research Quality in the DDD Literature
The analyzed studies demonstrate varying degrees of adherence to DDD principles and the effective application of DDD concepts. While some studies effectively apply DDD principles, others exhibit shortcomings in implementation and evaluation. To enhance research quality, future studies should emphasize the consistent use of Ubiquitous Language, explicit definition of Bounded Contexts, proper utilization of Domain Events and DDD patterns, and robust evaluation methodologies. Additionally, studies should openly discuss the challenges and limitations of DDD, contributing to a more comprehensive understanding of its potential and practical implications in software development.
Several studies employed the notion of Ubiquitous Language, which facilitates effective communication between stakeholders and software designers by creating a shared vocabulary. However, a few studies failed to implement this language consistently, potentially hindering communication and understanding among team members. Future research should emphasize the importance of adopting and consistently applying Ubiquitous Language throughout the development process to ensure better alignment and clarity.
Bounded Context, another key concept in DDD, helps define explicit boundaries where specific domain models apply [2, 21]. While some studies effectively utilized Bounded Context to organize and segregate domains, others did not explicitly mention or implement this principle. Ensuring clear identification and definition of Bounded Contexts is crucial for managing interactions between different models and contexts, and future studies should focus on this aspect to enhance research quality.
The application of Domain Events to trigger behavior across multiple aggregates within a Bounded Context was observed in some studies. However, not all studies effectively leveraged this concept. Implementing Domain Events can improve event-driven communication and coordination between domain components, promoting a more cohesive and flexible architecture [2, 21]. Future research should explore the potential benefits of Domain Events in event-driven systems and provide concrete examples of their implementation.
Several studies showcased the use of Aggregates, Entities, and Value Objects to model complex business domains. While these concepts were well-implemented in some cases, others lacked specific details or provided inadequate explanations about their usage. To bolster research quality, studies should strive for comprehensive and consistent implementation of these DDD patterns, ensuring a clear understanding of their roles and relationships within the domain model.
We observed that Strategic Design patterns such as Core Domain, Shared Kernel, and upstream–downstream relationships were underreported in most studies. This suggests a possible gap in the literature, where tactical DDD patterns are emphasized, but strategic alignment is less frequently explored or documented. Future research should pay more attention to these higher-level design patterns to understand their role in system architecture and team collaboration.
One significant aspect that emerged from the data was the lack of specific evaluation methods in some studies. While some research incorporated empirical evaluations, others relied solely on proposing methodologies or demonstrating case studies without conducting rigorous evaluations. Future studies should adopt more robust evaluation methods, such as controlled experiments, surveys, or user feedback, to assess the effectiveness of DDD implementation and provide empirical evidence to support their findings.
Moreover, most studies did not discuss the challenges and limitations associated with DDD adoption, leaving potential gaps in addressing concerns or drawbacks of the approach. To improve research quality, studies should acknowledge and discuss both the advantages and limitations of DDD, offering a balanced view to readers and practitioners. This will help in understanding the practical implications of DDD and guide future research and implementation efforts.
Future research can provide a more comprehensive and balanced analysis of DDD by addressing these areas to contribute to the advancement of knowledge and practice in the field of software development.




6 Implications
In this section, we introduce the implications of our findings for both practitioners and researchers. We draw on the results presented in our RQs to ground our implications in the evidence synthesized from the primary studies.
Implications for Practitioners.
One of the primary implications for practitioners is the recognition of the diverse benefits that DDD can bring to various software systems. The application of DDD principles has been shown to improve the modularity, maintainability, and scalability of microservices-based applications (Section 4.3), thereby enabling organizations to handle complex and distributed systems more effectively [24, 27, 32]. Practitioners can leverage these insights to enhance their software architecture and development practices by adopting DDD patterns such as Bounded Contexts and Aggregates to define clear service boundaries and promote decoupling.
Moreover, the adoption of DDD in enterprise software systems has proven valuable in addressing challenges related to legacy system modernization and increasing architectural complexity [25, 26, 31]. Section 4.2 illustrates how DDD facilitates flexible and maintainable architectures by aligning software components with evolving business needs. Practitioners aiming to modernize legacy systems should consider using context maps and ACLs (as highlighted in Section 4.5) to incrementally isolate and refactor legacy code while preserving domain integrity.
Additionally, DDD has demonstrated benefits in improving code quality and software construction in web applications and development tools [30, 28, 34]. In Section 4.3, studies report that domain annotations and modeling tools help practitioners maintain separation of concerns and automate parts of the development process. These insights can guide practitioners in selecting DDD-aligned tools that improve code maintainability [30] and streamline software generation.
However, DDD is not without its challenges. As discussed in Section 4.5, one of the most significant obstacles is the steep learning curve, especially for developers new to DDD [48, 46]. Misunderstandings in the application of patterns, unclear decomposition strategies, and the misalignment between teams and domains are common pitfalls. To mitigate this, practitioners should invest in onboarding initiatives, including dedicated DDD training sessions, domain modeling workshops (e.g., Event Storming [31]), and mentorship from experienced architects (see Table 8).
Implications for Researchers.
Despite the progress made in understanding the application and benefits of DDD, Section 4.6 shows that only a limited number of studies (approximately 39%) employ empirical evaluations. Future research should prioritize the design of empirical studies with concrete evaluation metrics—such as team productivity, domain alignment, and architectural complexity—to measure the actual impact of DDD adoption in industrial settings [24, 25, 31].
Furthermore, Section 4.5 indicates that the complexity of managing domain boundaries, integrating DDD with legacy systems, and supporting DSL-based tooling are persistent technical challenges. Researchers should explore novel techniques—e.g., automated decomposition, model-code synchronization, and refactoring strategies—to support scalable DDD adoption in complex environments [36, 48].
Another important research direction involves studying stakeholder dynamics. Section 4.7 emphasizes the critical role of collaboration between software engineers, architects, and domain experts in the success of DDD. Yet, communication breakdowns and a lack of domain understanding remain major barriers [51, 45]. Researchers could investigate socio-technical methods (e.g., participatory modeling [66, 67], collaborative tooling [68]) to bridge this gap and formalize co-design practices.
In addition, Section 4.6 reveals that studies vary widely in how they evaluate DDD. Developing standardized frameworks and reproducible evaluation methodologies will help unify the field and allow comparison across contexts [48, 30]. Incorporating both technical metrics (e.g., coupling, cohesion) and human-centered metrics (e.g., developer perception, onboarding effort) can lead to a more holistic understanding of DDD’s effectiveness.
Based on our findings, future research should prioritize empirical studies that not only report qualitative insights but also collect large-scale (e.g. via repository mining for DDD similar to [69]), longitudinal and team-level data. While several studies use focus groups and interviews, we observed a lack of sustained evaluations on aspects such as maintainability, modularity drift over time, or onboarding effectiveness. Studies like Braun et. al. [36, 38] and Özkan et. al. [48] demonstrate the potential of combining qualitative feedback with structured metrics, yet these efforts remain isolated. To build on this, researchers could design action research or longitudinal case studies that track DDD adoption over multiple sprints or releases, focusing on how domain alignment evolves, how design debt is managed, and how development teams adapt. Similarly, design-science-based evaluations of modeling tools (e.g., Context Mapper) can shed light on how tooling impacts the domain-code connection in practical workflows. These directions would help close both methodological and tooling-related gaps identified in current literature.
By addressing these research gaps, scholars can contribute not only to the theoretical advancement of DDD but also to practical guidelines, tool support, and empirical validation frameworks that help teams successfully adopt DDD in complex software projects.



7 Limitations and Threats to Validity
Some limitations and validity considerations are applicable for SLR studies [70, 71]. The limitations and threats to the validity of this study are discussed in this section.
Timeframe and Currency. One limitation is the timeframe of the literature search and the currency of the included studies. The SLR is based on data available up to June 2023, and relevant studies published after that date might have been missed. The field of DDD is rapidly evolving, and newer developments or trends may not be fully captured in this review.
Quality of Studies. The quality of individual studies can impact the overall reliability of the SLR findings. The included studies vary in terms of methodological rigor, sample size, and research design. Some studies might have limitations or biases that could influence their results and subsequent conclusions. Nonetheless, we endeavored to rigorously exclude studies of low quality by systematically applying inclusion and exclusion criteria as well as conducting thorough quality assessments.
Inclusion and Exclusion Criteria.
The effectiveness of an SLR largely depends on the clarity and appropriateness of the inclusion and exclusion criteria. Despite our efforts to define comprehensive criteria, there is a possibility of unintentionally excluding relevant studies or including studies that do not precisely fit the research questions. To mitigate this potential limitation, a rigorous approach was adopted. Two authors independently applied predefined inclusion and exclusion criteria to a randomly selected pool of 60 studies. The level of agreement between the two authors was assessed using Cohen’s Kappa coefficient [18], a widely accepted measure of inter-rater reliability. Encouragingly, the analysis revealed a perfect agreement between the authors in their decisions regarding study inclusion and exclusion within the randomly sampled studies. This robust agreement underscores the reliability of the selection process and enhances the credibility of the findings.
Bias in Data Extraction. A limitation related to the validity of data extraction in this study is the potential for human error or bias during the data extraction process. Despite efforts to maintain consistency and accuracy, the interpretation of information from primary studies could be influenced by the subjectivity of the researchers. Different individuals extracting data may interpret the findings differently or unintentionally introduce errors, leading to inconsistencies in the extracted data. To address this limitation, a careful and systematic approach was employed, and multiple researchers were involved in the data extraction process. Inter-rater reliability checks and discussions among the researchers were conducted to ensure agreement and minimize discrepancies. However, it is essential to acknowledge that the possibility of human-related errors cannot be entirely eliminated, and they may have some impact on the final results and conclusions of the systematic literature review.
Publication Bias. One potential threat to validity is publication bias. The studies included in this SLR were obtained from various academic databases and sources, which might not represent the entire body of literature on DDD. Some studies might not have been published or accessible, leading to a skewed representation of the research landscape. Nevertheless, to mitigate this bias, we performed a comprehensive search across multiple academic databases, including Google Scholar and Scopus during the snowballing process. This extensive search approach was aimed at encompassing a wide range of academic sources and reducing potential biases in the selection of studies.
Language Bias. The SLR focused primarily on studies published in English, which might introduce a language bias. Relevant research conducted in other languages could have been omitted, potentially impacting the comprehensiveness of the review.
Agreement Level During Quality Assessment. Another potential threat to validity is the lack of assessment of agreement levels between assessors during the quality assessment stage. While Cohen’s Kappa was used to evaluate inter-rater reliability during the inclusion/exclusion stage, no similar measure was applied during the quality assessment of the included studies. This omission could introduce subjectivity and potential bias in the quality assessment process, as individual assessors may have differing interpretations of quality criteria. However, during the quality assessment stage, in cases of disagreement or doubt, the assessors consulted with each other to reach a consensus. This approach used to ensure that the quality assessments were rigorous, minimized potential biases, and allowed discrepancies to be resolved systematically.
Study-Specific Validity Considerations.
In addition to general methodological considerations, we acknowledge several threats specific to our study. First, our focus on implementation-related search terms (e.g., refactor*, improv*, optimiz*) may have led to the exclusion of relevant DDD papers that are more theoretical or descriptive in nature. This trade-off was a deliberate decision to ensure practical relevance, but it may affect the completeness of our study population. Second, while our inclusion of forward snowballing from Evans’ seminal book aimed to enrich the dataset with practically grounded work, it may also have introduced a bias toward studies that cite this particular source, possibly underrepresenting alternative DDD schools of thought. Finally, our coding and classification of DDD patterns — especially in distinguishing strategic vs. tactical applications — required interpretation and contextual judgment. Despite using a predefined form and multiple authors reviewing the coding, subjectivity may still have influenced the final categorization.



8 Conclusion
This SLR provides a comprehensive overview of the effectiveness, challenges, and implications of adopting DDD in software development. Through the analysis of a diverse set of primary studies, we identified key stakeholders involved in the implementation of DDD, including Software Engineers, Architects, Project Managers, and Domain Experts. These stakeholders play critical roles in ensuring the successful application of DDD principles and practices in various software systems.
The findings highlight the significance of Ubiquitous Language as a core principle of DDD, emphasizing its role in facilitating effective communication and collaboration among team members. Bounded Contexts and the decomposition of microservices stand out as fundamental concepts in DDD, enabling the creation of scalable and maintainable software systems. Domain Services, Entities, and Value Objects serve as tactical DDD patterns, contributing to the modeling of complex business domains.
The implementation of DDD demands considerable attention and effort, particularly in transitioning from traditional software development approaches to DDD. Experienced developers and domain experts play crucial roles in effectively implementing DDD concepts. The involvement of domain experts is especially critical for crafting domain models that accurately represent real-world domain requirements.
The SLR also highlights the rise of microservices in the context of DDD, indicating a significant shift in the popularity of microservices-based architectures since 2017. However, further empirical research is needed to comprehensively understand the benefits and challenges of implementing DDD in different software development scenarios.
The collaboration between academia and industry in DDD research is evident, with academic studies contributing theoretical advancements and innovative approaches, while industry-focused studies showcase practical implementation and real-world applicability. This collaboration can facilitate knowledge transfer and best practices, contributing to the advancement and adoption of DDD principles in software development.
A critical analysis of research quality in the DDD literature reveals varying degrees of adherence to DDD principles and the effective application of DDD concepts. Future studies should emphasize the consistent use of Ubiquitous Language, clear definition of Bounded Contexts, proper utilization of Domain Events and DDD patterns, and robust evaluation methodologies. Moreover, studies should openly discuss the challenges and limitations of DDD to provide a comprehensive understanding of its potential and practical implications.
DDD offers valuable insights and practices for software development, fostering communication, and collaboration among stakeholders while modeling complex business domains. However, its successful implementation depends on the expertise of stakeholders, proper utilization of DDD principles, and the adoption of robust evaluation methods. As software systems continue to grow in complexity, DDD remains a relevant and promising approach for developing domain-centric and scalable software solutions.


Declaration of Competing Interest
The authors declare that they have no known competing financial interests or personal relationships that could have appeared to influence the work reported in this paper.


Data Availability
Data will be made available on request.


References


Avram and Marinescu [2006]

A. Avram, F. Marinescu,
Domain-Driven Design Quickly: A Summary of Eric
Evans’ Domain-Driven Design, Enterprise Software Development
Series, C4Media, 2006.




Evans [2004]

E. Evans, Domain-Driven Design: Tackling
Complexity in the Heart of Software, Addison-Wesley,
Boston, 2004.




Vernon [2013]

V. Vernon, Implementing Domain-Driven
Design, Addision-Wesley, Upper Saddle
River, NJ, 2013.




Kitchenham and Charters [2007]

B. Kitchenham, S. Charters,


Guidelines for performing Systematic Literature
Reviews in Software Engineering 2
(2007).




Wohlin [2014]

C. Wohlin,


Guidelines for snowballing in systematic literature
studies and a replication in software engineering,


in: Proceedings of the 18th International
Conference on Evaluation and Assessment in Software
Engineering, ACM, London England
United Kingdom, 2014, pp. 1–10.
doi:10.1145/2601248.2601268.




Gamma et al. [2009]

E. Gamma, R. Helm, R. E.
Johnson, J. Vlissides, Design patterns:
elements of reusable object-oriented software,
Addison-Wesley, Reading, MA,
2009. OCLC: 624525693.




Buschmann [1996]

F. Buschmann (Ed.), Pattern-oriented software
architecture: a system of patterns, Wiley,
Chichester ; New York, 1996.




Brandolini [2013]

A. Brandolini, Introducing eventstorming –
an explosive modeling technique for lean discovery,
https://ziobrando.blogspot.com/2013/11/introducing-eventstorming.html,
2013. Accessed April 2025.




Le et al. [2016]

D. M. Le, D.-H. Dang,
V.-H. Nguyen,


Domain-driven design patterns: A metadata-based
approach,


in: 2016 IEEE RIVF International Conference
on Computing & Communication Technologies, Research,
Innovation, and Vision for the Future (RIVF),
IEEE, Hanoi, 2016,
pp. 247–252. doi:10.1109/RIVF.2016.7800302.




Singjai et al. [2021]

A. Singjai, U. Zdun,
O. Zimmermann,


Practitioner Views on the Interrelation of
Microservice APIs and Domain-Driven Design: A Grey Literature Study
Based on Grounded Theory,


in: 2021 IEEE 18th International
Conference on Software Architecture (ICSA),
IEEE, Stuttgart, Germany,
2021, pp. 25–35.
doi:10.1109/ICSA51549.2021.00011.




De Goey et al. [2019]

H. De Goey, P. Hilletofth,
L. Eriksson,


Design-driven innovation: A systematic literature
review,


European Business Review 31
(2019) 92–114.
doi:10.1108/EBR-09-2017-0160.




Bertoni [2020]

A. Bertoni,


DATA-DRIVEN DESIGN IN CONCEPT DEVELOPMENT:
SYSTEMATIC REVIEW AND MISSED OPPORTUNITIES,


Proceedings of the Design Society: DESIGN
Conference 1 (2020)
101–110. doi:10.1017/dsd.2020.4.




Stol and Fitzgerald [2018]

K.-J. Stol, B. Fitzgerald,


The ABC of software engineering research,


ACM Transactions on Software Engineering and
Methodology (TOSEM) 27 (2018)
1–51.




Lübke et al. [2019]

D. Lübke, O. Zimmermann,
C. Pautasso, U. Zdun,
M. Stocker,


Interface Evolution Patterns: Balancing
Compatibility and Extensibility across Service Life Cycles,


in: Proceedings of the 24th European
Conference on Pattern Languages of Programs, EuroPLop ’19,
Association for Computing Machinery,
New York, NY, USA, 2019.
doi:10.1145/3361149.3361164.




Giray et al. [2023]

G. Giray, K. E. Bennin,
Ö. Köksal, Ö. Babur,
B. Tekinerdogan,


On the use of deep learning in software defect
prediction,


Journal of Systems and Software
195 (2023) 111537.
URL: https://linkinghub.elsevier.com/retrieve/pii/S0164121222002138.
doi:10.1016/j.jss.2022.111537.




Ochoa et al. [2025]

L. Ochoa, M. Hammad,
G. Giray, Ö. Babur,
K. Bennin,


Characterising harmful api uses and repair
techniques: Insights from a systematic review,


Computer Science Review 57
(2025) 100732.




Kuhrmann et al. [2017]

M. Kuhrmann, D. M. Fernández,
M. Daneva,


On the pragmatic design of literature studies in
software engineering: An experience-based guideline,


Empirical Software Engineering
22 (2017) 2852–2891.
doi:10.1007/s10664-016-9492-y.




Cohen [1960]

J. Cohen,


A Coefficient of Agreement for Nominal
Scales,


Educational and Psychological Measurement
20 (1960) 37–46.
doi:10.1177/001316446002000104.




Torres et al. [2021]

W. Torres, M. G. J. Van Den Brand,
A. Serebrenik,


A systematic literature review of cross-domain model
consistency checking by model management tools,


Software and Systems Modeling 20
(2021) 897–916.
doi:10.1007/s10270-020-00834-1.




Wang et al. [2023]

S. Wang, L. Huang,
A. Gao, J. Ge,
T. Zhang, H. Feng,
I. Satyarth, M. Li,
H. Zhang, V. Ng,


Machine/Deep Learning for Software
Engineering: A Systematic Literature Review,


IEEE Transactions on Software Engineering
49 (2023) 1188–1231.
doi:10.1109/TSE.2022.3173346.




Vernon [2016]

V. Vernon, Domain-Driven Design Distilled,
Addison-Wesley, Boston,
2016.




Nilsson [2006]

J. Nilsson, Applying Domain-Driven Design and
Patterns: With Examples in C# and .NET,
Addison-Wesley, Upper Saddle River,
NJ, 2006.




Popay et al. [2006]

J. Popay, H. Roberts,
A. Sowden, M. Petticrew,
L. Arai, M. Rodgers,
N. Britten, K. Roen,
S. Duffy, Guidance on the Conduct of
Narrative Synthesis in Systematic Reviews: A Product from the ESRC
Methods Programme, Lancaster University,
2006. doi:10.13140/2.1.1018.4643.




Singjai et al. [2021]

A. Singjai, U. Zdun,
O. Zimmermann, C. Pautasso,


Patterns on Deriving APIs and their Endpoints
from Domain Models,


in: 26th European Conference on Pattern
Languages of Programs, ACM,
Graz Austria, 2021, pp.
1–15. doi:10.1145/3489449.3489976.




Maddodi et al. [2020]

G. Maddodi, S. Jansen,
M. Overeem,


Aggregate Architecture Simulation in
Event-Sourcing Applications using Layered Queuing Networks,


in: Proceedings of the ACM/SPEC
International Conference on Performance Engineering,
ACM, Edmonton AB Canada,
2020, pp. 238–245.
doi:10.1145/3358960.3375797.




Peng and Hu [2007]

S. Peng, Y. Hu,


IAnticorruption: A domain-driven design approach
to more robust integration,


in: Companion to the 22nd ACM SIGPLAN
Conference on Object-oriented Programming Systems and Applications
Companion, ACM, Montreal Quebec
Canada, 2007, pp. 976–982.
doi:10.1145/1297846.1297966.




Josélyne et al. [2018]

M. I. Josélyne, D. Tuheirwe-Mukasa,
B. Kanagwa, J. Balikuddembe,


Partitioning microservices: A domain engineering
approach,


in: Proceedings of the 2018 International
Conference on Software Engineering in Africa,
ACM, Gothenburg Sweden,
2018, pp. 43–49.
doi:10.1145/3195528.3195535.




Perillo et al. [2009]

J. R. C. Perillo, E. M. Guerra,
C. T. Fernandes,


Daileon: A tool for enabling domain annotations,


in: Proceedings of the Workshop on AOP
and Meta-Data for Software Evolution, ACM,
Genova Italy, 2009, pp.
1–4. doi:10.1145/1562860.1562867.




Le et al. [2019]

D. M. Le, D.-H. Dang,
H. T. Vu,


jDomainApp: A Module-Based Domain-Driven
Software Framework,


in: Proceedings of the Tenth International
Symposium on Information and Communication Technology - SoICT
2019, ACM Press, Hanoi, Ha Long Bay,
Viet Nam, 2019, pp. 399–406.
doi:10.1145/3368926.3369657.




Le et al. [2022]

D. M. Le, A. P. Nguyen,
L. Q. Tran, H. T. Le,
H. V.-A. Tong,


Generating Multi-platform Single Page
Applications: A Hierarchical Domain-Driven Design Approach,


in: The 11th International Symposium on
Information and Communication Technology, ACM,
Hanoi Vietnam, 2022, pp.
344–351. doi:10.1145/3568562.3568566.




Uludağ et al. [2018]

Ö. Uludağ, M. Hauder,
M. Kleehaus, C. Schimpfle,
F. Matthes,


Supporting Large-Scale Agile Development with
Domain-Driven Design,


in: J. Garbajosa, X. Wang,
A. Aguiar (Eds.), Agile Processes
in Software Engineering and Extreme Programming, volume
314, Springer International
Publishing, Cham, 2018, pp.
232–247. doi:10.1007/978-3-319-91602-6_16.




Wang et al. [2022]

Y. Wang, S. Li, H. Liu,
H. Zhang, B. Pan,


A Reference Architecture for Blockchain-based
Traceability Systems Using Domain-Driven Design and Microservices,


in: 2022 29th Asia-Pacific Software Engineering
Conference (APSEC), IEEE,
Japan, 2022, pp.
269–278. doi:10.1109/APSEC57359.2022.00039.




Landre et al. [2006]

E. Landre, H. Wesenberg,
H. Rønneberg,


Architectural improvement by use of strategic level
domain-driven design,


in: Companion to the 21st ACM SIGPLAN
Symposium on Object-oriented Programming Systems, Languages, and
Applications, ACM, Portland Oregon
USA, 2006, pp. 809–814.
doi:10.1145/1176617.1176728.




Kapferer and Zimmermann [2020]

S. Kapferer, O. Zimmermann,


Domain-driven service design,


in: Service-Oriented Computing,
Communications in Computer and Information Science,
Springer International Publishing,
Cham, 2020, pp. 189–208.




Camilli et al. [2023]

M. Camilli, C. Colarusso,
B. Russo, E. Zimeo,


Actor-driven Decomposition of Microservices
through Multi-level Scalability Assessment,


ACM Transactions on Software Engineering and
Methodology (2023) 3583563.
doi:10.1145/3583563.




Braun et al. [2021]

S. Braun, S. Deßloch,
E. Wolff, F. Elberzhager,
A. Jedlitschka,


Tackling Consistency-related Design Challenges of
Distributed Data-Intensive Systems: An Action Research Study,


in: Proceedings of the 15th ACM / IEEE
International Symposium on Empirical Software Engineering and
Measurement (ESEM), ACM, Bari
Italy, 2021, pp. 1–11.
doi:10.1145/3475716.3475771.




Landre et al. [2007]

E. Landre, H. Wesenberg,
J. Olmheim,


Agile enterprise software development using
domain-driven design and test first,


in: Companion to the 22nd ACM SIGPLAN
Conference on Object-oriented Programming Systems and Applications
Companion, ACM, Montreal Quebec
Canada, 2007, pp. 983–993.
doi:10.1145/1297846.1297967.




Braun et al. [2021]

S. Braun, A. Bieniusa,
F. Elberzhager,


Advanced Domain-Driven Design for Consistency
in Distributed Data-Intensive Systems,


in: Proceedings of the 8th Workshop on
Principles and Practice of Consistency for Distributed Data,
ACM, Online United Kingdom,
2021, pp. 1–12.
doi:10.1145/3447865.3457969.




Zhao and Zhao [2021]

J. Zhao, K. Zhao,


Applying Microservice Refactoring to
Object-2riented Legacy System,


in: 2021 8th International Conference on
Dependable Systems and Their Applications (DSA),
IEEE, Yinchuan, China,
2021, pp. 467–473.
doi:10.1109/DSA52907.2021.00070.




Krause et al. [2020]

A. Krause, C. Zirkelbach,
W. Hasselbring, S. Lenga,
D. Kroger,


Microservice Decomposition via Static and
Dynamic Analysis of the Monolith,


in: 2020 IEEE International Conference on
Software Architecture Companion (ICSA-C), IEEE,
Salvador, Brazil, 2020, pp.
9–16. doi:10.1109/ICSA-C50368.2020.00011.




Snoeck and Wautelet [2022]

M. Snoeck, Y. Wautelet,


Agile MERODE: A model-driven software engineering
method for user-centric and value-based development,


Software and Systems Modeling 21
(2022) 1469–1494.
doi:10.1007/s10270-022-01015-y.




Danenas and Garsva [2012]

P. Danenas, G. Garsva,


Domain Driven Development and Feature Driven
Development for Development of Decision Support Systems,


in: T. Skersys, R. Butleris,
R. Butkiene (Eds.), Information and
Software Technologies, volume 319,
Springer Berlin Heidelberg, Berlin,
Heidelberg, 2012, pp. 187–198.
doi:10.1007/978-3-642-33308-8_16.




Kapferer and Zimmermann [2021]

S. Kapferer, O. Zimmermann,


Domain-Driven Architecture Modeling and Rapid
Prototyping with Context Mapper,


in: S. Hammoudi, L. F. Pires,
B. Selić (Eds.), Model-Driven
Engineering and Software Development, volume 1361,
Springer International Publishing,
Cham, 2021, pp. 250–272.
doi:10.1007/978-3-030-67445-8_11.




Hammarström and Herzog [2016]

P. Hammarström, E. Herzog,


Experience from integrating Domain Driven Software
System Design into a Systems Engineering Organization,


INCOSE International Symposium
26 (2016) 1192–1203.
doi:10.1002/j.2334-5837.2016.00220.x.




Da Silva et al. [2022]

C. E. Da Silva, E. L. Gomes,
S. S. Basu,


BPM2DDD: A Systematic Process for
Identifying Domains from Business Processes Models,


Software 1
(2022) 417–449.
doi:10.3390/software1040018.




Hippchen et al. [2017]

B. Hippchen, P. Giessler,
R. H. Steinegger, M. Schneider,
S. Abeck,


Designing Microservice-Based Applications by
Using a Domain-Driven Design Approach (2017).




Ding et al. [2020]

Y. Ding, L. Wang, S. Li,
X. Wang, J. Zhang,


Enterprise service application architecture based on
Domain Driven Model Design,


in: 2020 2nd International Conference on
Information Technology and Computer Application (ITCA),
IEEE, Guangzhou, China,
2020, pp. 778–784.
doi:10.1109/ITCA52113.2020.00167.




Özkan et al. [2023]

O. Özkan, Ö. Babur,
M. Van Den Brand,


Refactoring with domain-driven design in an
industrial context: An action research report,


Empirical Software Engineering
28 (2023) 94.
doi:10.1007/s10664-023-10310-1.




Pereira and Silva [2022]

P. Pereira, A. R. Silva,
Towards Transactional Causal Consistent Microservices
Business Logic, 2022.
arXiv:2212.11658.




Joselyne et al. [2021]

M. I. Joselyne, G. Bajpai,
F. Nzanywayingoma,


A Systematic Framework of Application
Modernization to Microservice based Architecture,


in: 2021 International Conference on
Engineering and Emerging Technologies (ICEET),
IEEE, Istanbul, Turkey,
2021, pp. 1–6.
doi:10.1109/ICEET53442.2021.9659783.




Wanderley and Da Silveria [2012]

F. Wanderley, D. S. Da Silveria,


A Framework to Diminish the Gap between
the Business Specialist and the Software Designer,


in: 2012 Eighth International Conference on
the Quality of Information and Communications Technology,
IEEE, Lisbon, TBD, Portugal,
2012, pp. 199–204.
doi:10.1109/QUATIC.2012.9.




Koryl [2017]

M. Koryl,


Active resources concept of computation for
enterprise software,


Archives of Control Sciences 27
(2017) 279–291.
doi:10.1515/acsc-2017-0018.




Mayer and Weinreich [2018]

B. Mayer, R. Weinreich,


An Approach to Extract the Architecture
of Microservice-Based Software Systems,


in: 2018 IEEE Symposium on Service-Oriented
System Engineering (SOSE), IEEE,
Bamberg, 2018, pp.
21–30. doi:10.1109/SOSE.2018.00012.




Vural and Koyuncu [2021]

H. Vural, M. Koyuncu,


Does Domain-Driven Design Lead to Finding the
Optimal Modularity of a Microservice?,


IEEE Access 9
(2021) 32721–32733.
doi:10.1109/ACCESS.2021.3060895.




Oukes et al. [2021]

P. Oukes, M. V. Andel,
E. Folmer, R. Bennett,
C. Lemmen,


Domain-Driven Design applied to land
administration system development: Lessons from the Netherlands,


Land Use Policy 104
(2021) 105379.
doi:10.1016/j.landusepol.2021.105379.




Häser et al. [2016]

F. Häser, M. Felderer,
R. Breu,


Is business domain language support beneficial for
creating test case specifications: A controlled experiment,


Information and Software Technology
79 (2016) 52–62.
doi:10.1016/j.infsof.2016.07.001.




Le et al. [2018]

D. M. Le, D.-H. Dang,
V.-H. Nguyen,


On domain driven design using annotation-based domain
specific language,


Computer Languages, Systems & Structures
54 (2018) 199–235.
doi:10.1016/j.cl.2018.05.001.




Abeck et al. [2019]

S. Abeck, M. Schneider,
J.-P. Quirmbach, H. Klarl,
C. Urbaczek, S. Zogaj,


A Context Map as the Basis for a
Microservice Architecture for the Connected Car Domain
(2019). doi:10.18420/INF2019_18.




Bünder and Kuchen [2019]

H. Bünder, H. Kuchen,


A model-driven approach for behavior-driven GUI
testing,


in: Proceedings of the 34th ACM/SIGAPP
Symposium on Applied Computing, ACM,
Limassol Cyprus, 2019, pp.
1742–1751. doi:10.1145/3297280.3297450.




Dragoni et al. [2017]

N. Dragoni, S. Giallorenzo,
A. L. Lafuente, M. Mazzara,
F. Montesi, R. Mustafin,
L. Safina, Microservices: Yesterday, today,
and tomorrow, 2017.
doi:10.48550/arXiv.1606.04036.
arXiv:1606.04036.




Fritzsch et al. [2019]

J. Fritzsch, J. Bogner,
S. Wagner, A. Zimmermann,


Microservices Migration in Industry:
Intentions, Strategies, and Challenges,


in: 2019 IEEE International Conference on
Software Maintenance and Evolution (ICSME),
IEEE, Cleveland, OH, USA,
2019, pp. 481–490.
doi:10.1109/ICSME.2019.00081.




Lewis and Fowler [2014]

J. Lewis, M. Fowler,


Microservices: A definition of this new architectural
term,


MartinFowler. com 25
(2014) 12.




Hamzehloui et al. [2018]

M. S. Hamzehloui, S. Sahibuddin,
K. Salah,


A Systematic Mapping Study on Microservices,


in: Advances in Intelligent Systems and
Computing, Springer International Publishing,
2018, pp. 1079–1090.
doi:10.1007/978-3-319-99007-1_100.




Viggiato et al. [2018]

M. Viggiato, R. Terra,
H. Rocha, M. T. Valente,
E. Figueiredo,


Microservices in Practice: A Survey Study
(2018). doi:10.48550/ARXIV.1808.04836.




Knoche and Hasselbring [2019]

H. Knoche, W. Hasselbring,


Drivers and Barriers for Microservice
Adoption – A Survey among Professionals in Germany,


Enterprise Modelling and Information Systems
Architectures (EMISAJ) (2019) 1:1–35
Pages. doi:10.18417/EMISA.14.1.




Voinov and Bousquet [2010]

A. Voinov, F. Bousquet,


Modeling with stakeholders,


Environmental Modelling & Software
25 (2010) 1268–1281.




Brandolini [2019]

A. Brandolini, Introducing EventStorming: An
act of deliberate collective learning, Leanpub,
2019.




Famelis et al. [2012]

M. Famelis, R. Salay,
M. Chechik,


Partial models: Towards modeling and reasoning with
uncertainty,


in: Proceedings of the 34th International
Conference on Software Engineering (ICSE), IEEE,
2012, pp. 573–583.




Babur et al. [2024]

Ö. Babur, E. Constantinou,
A. Serebrenik,


Language usage analysis for emf metamodels on
github,


Empirical Software Engineering
29 (2024) 23.




Petersen et al. [2015]

K. Petersen, S. Vakkalanka,
L. Kuzniarz,


Guidelines for conducting systematic mapping studies
in software engineering: An update,


Information and Software Technology
64 (2015) 1–18.
doi:10.1016/j.infsof.2015.03.007.




Petersen et al. [2008]

K. Petersen, R. Feldt,
S. Mujtaba, M. Mattsson,


Systematic Mapping Studies in Software
Engineering,


in: 12th International Conference on
Evaluation and Assessment in Software Engineering (EASE),
2008. doi:10.14236/ewic/EASE2008.8.







