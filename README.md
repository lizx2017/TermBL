# Locating Faulty File of Bug Report with Distinction
Software bugs can cause disastrous consequences. To improve software quality, the issue tracking system is provided for users to reports bugs and then programmers have to carefully read bug reports and locate the faulty files. The labor consumption rises with the scale of the software. To reduce the effort, automatic faulty file localization has long been a hot research topic.

Given a bug report, the state-of-the-art approaches compare the bug report with the source files of a buggy version, which are known as IR-based methods. However, we argue that when a bug report is filed by users, it is often infeasible to recreate its buggy version. Researchers typically regard the commit right before a bug fix as the buggy version of the bug, which is usually different with the real one when users report the bug. As a result, for most bug reports, the prior approaches were evaluated on unreal inputs.

To handle the above-mentioned problem, we propose DISBL, the first approach, which extracts the distinctions of all files and uses such distinctions to locate the faulty files of a new bug report. For a new bug report, the distinctions are extracted from all the previously fixed issue reports before it. The approach avoid the recreation of the buggy version, because it does not use the source files of the buggy version as its inputs.

## Datasets
<table>
   <tr>
      <td>project</td>
      <td># of commits</td>
      <td></td>
      <td></td>
      <td># of issues</td>
      <td></td>
      <td></td>
      <td># of bugs</td>
      <td></td>
      <td></td>
      <td># of files</td>
   </tr>
   <tr>
      <td></td>
      <td>linked</td>
      <td>total</td>
      <td>%</td>
      <td>linked</td>
      <td>total</td>
      <td>%</td>
      <td>linked</td>
      <td>total</td>
      <td>%</td>
      <td></td>
   </tr>
   <tr>
      <td>GROOVY</td>
      <td>6012</td>
      <td>16973</td>
      <td>35.4%</td>
      <td>4412</td>
      <td>8623</td>
      <td>51.2%</td>
      <td>2802</td>
      <td>5790</td>
      <td>48.4%</td>
      <td>1032</td>
   </tr>
   <tr>
      <td>PDFBOX</td>
      <td>8452</td>
      <td>8782</td>
      <td>96.2%</td>
      <td>2572</td>
      <td>4775</td>
      <td>53.9%</td>
      <td>1720</td>
      <td>3425</td>
      <td>50.2%</td>
      <td>1122</td>
   </tr>
   <tr>
      <td>CARBONDATA</td>
      <td>2547</td>
      <td>4391</td>
      <td>58.0%</td>
      <td>2331</td>
      <td>3576</td>
      <td>65.2%</td>
      <td>1182</td>
      <td>1948</td>
      <td>60.7%</td>
      <td>1034</td>
   </tr>
   <tr>
      <td>AMQ</td>
      <td>5662</td>
      <td>10652</td>
      <td>53.2%</td>
      <td>1218</td>
      <td>7057</td>
      <td>17.3%</td>
      <td>775</td>
      <td>4773</td>
      <td>16.2%</td>
      <td>851</td>
   </tr>
   <tr>
      <td>ARIES</td>
      <td>3085</td>
      <td>5702</td>
      <td>54.1%</td>
      <td>497</td>
      <td>1931</td>
      <td>25.7%</td>
      <td>878</td>
      <td>1141</td>
      <td>77.0%</td>
      <td>1194</td>
   </tr>
   <tr>
      <td>UIMA</td>
      <td>14911</td>
      <td>18231</td>
      <td>81.8%</td>
      <td>4731</td>
      <td>6146</td>
      <td>77.0%</td>
      <td>2653</td>
      <td>3372</td>
      <td>78.7%</td>
      <td>1467</td>
   </tr>
   <tr>
      <td>Total</td>
      <td>40669</td>
      <td>64731</td>
      <td>62.8%</td>
      <td>15761</td>
      <td>32108</td>
      <td>49.1%</td>
      <td>10010</td>
      <td>20449</td>
      <td>49.0%</td>
      <td>6700</td>
   </tr>
   <tr>
      <td></td>
   </tr>
</table>

Here are [Raw Datasets](./dataset) we use in this project.

## Distinctions
The distinctions of a file are words and code names that appear in its related issue reports. The distinctions can distinguish the symptom, cause, and functionality of the file from those of other files. 

In this project, we carefully separate the structured information, including code snippets, stack traces and API mentions apart from textual descriptions. And then, we follow the conventional natural language processing steps, such as splitting by white spaces, removing stopwords and extracting stems. Finally, we leverage TFIDF to extract the distinctions of each source files based on descriptions in preceding resolved issue reports. Here we exhibit the [distinctions](./distinction) of source files and the [stopwords](./stopwords/stopwords.txt) we use.

## Evaluation Results
#### Overall Effectiveness
The accuracy values of projects UIMA, GROOVY, PDFBOX and AMQ are around 20% with only one file recommended. With 5 files recommended, the accuracy values of UIMA, GROOVY and PDFBOX become around 50% and the average accuracy value of all projects raises to 42%. Our recommendations are provided in [results/recommendations](./results/recommendations).
In summary, even without the source files of buggy versions, DISBL achieved actuary values that are similar to IR-based approaches, and there is sufficient space for improvements.

#### Rankings of real faulty files
To figure out to what extent the real faulty files can be localized, we exhibit the rankings of them in the recommendation list in [result/rankings](./results/rankings). The real faulty files of quite a few bugs get top 5 rankings in the recommendation list, while some other files keep low rankings. Researchers proposed approaches to extract various contents from software engineering documents. If we use such approaches to identify the contents of issue reports, we can further improve the effectiveness of our approach.