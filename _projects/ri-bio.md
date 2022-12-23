---
layout: page
title: ri bio api search tool
description: Mar 2021 - Aug 2021
importance: 3
img: assets/img/ribiosquare.png
category: school/work
---

<h3>Background</h3>
During the spring of 2021, I joined Pangea, a freelancer network made up of college students. I was connected with RI Bio, a group aiming to improve the life sciences industry in Rhode Island. 

Prior to the development of this tool, researchers looked up the same term in 4 different databases to see what existed so far. RI Bio was looking for a more efficient search for their use cases - they wanted to receive results from all databases in one search, and be able to see titles, authors, and other general information. From this basic information, if something was intriguing, they could then go look it up in the specific database.

In additional to basic search results, there were some analyses that could give good signals on the search term.

**Field Expert**: Who's name pops up the most across papers, patents, grants, and trials? (This may potentially be a good person to reach out to)

**Patent Similarity**: Without needing to read every patent, what patents are focused on similar ideas and technologies?

Note: the researchers/users of this tool did not have a CS technical background, so the setup and usage of the tool could not be too technically involved.

<h3>Implementation</h3>

Django was used to build an API which would send requests to each of the database's APIs, compile and analyze the data, and output a excel file with multiple sheets.

The full source code can be found [here](https://github.com/jasmine2000/ri-bio-project), and there will be links to specific files below. 

<h4>Quick Overview</h4>

Implementation of this was a little tedious but fairly straightforward - it involved following the API docs for each database, writing requests in the correct format, and displaying relevant information in an excel sheet. The four databases queried from are shown below. They link to the source code used to connect to the specific API.

- [ClinicalTrials.gov](https://github.com/jasmine2000/ri-bio-project/blob/main/api_search_tool/my_project/views/ct.py) - clinical trials

- [NIH Federal Reporter](https://github.com/jasmine2000/ri-bio-project/blob/main/api_search_tool/my_project/views/nih.py) - projects granted federal money

- [Lens Scholarly Search](https://github.com/jasmine2000/ri-bio-project/blob/main/api_search_tool/my_project/views/lens_s.py) - scholarly publications

- [Lens Patent Search](https://github.com/jasmine2000/ri-bio-project/blob/main/api_search_tool/my_project/views/lens_p.py) - patents

<h4>Analysis - Field Expert</h4>
First, we would pull all names (authors, primary investigators, inventors, etc.). 

All the name patterns were slightly different (Last, First vs First Middle Last), but after transforming all the names into the same pattern, this was also fairly straightforward - a count was associated with each name, and the names were shown on a list in descending order, alongside the titles of the paper/patent/grant/trial they appeared in. 

The source code can be found [here](https://github.com/jasmine2000/ri-bio-project/blob/main/api_search_tool/my_project/views/authors.py). The "translator" dictionary at the beginning tracks the columns the names appear in, and the indices of the first and last name (databases that stored names as Last First would have indices 1, 0 in this case). The Lens databases are commented out because in the last iteration of this code, the Lens API key had expired.

<h4>Analysis - Patent Similarity</h4>
The process for comparing all patents is as follows:

1. For each patent, get the claims (what does this patent cover), and ignore common words (a, the, is, etc.).

2. Use set similarity search, comparing all possible pairs, to see which pairs of patents have the highest similarity.

Note - if you look at the [source code](https://github.com/jasmine2000/ri-bio-project/blob/main/api_search_tool/my_project/views/claims.py) for this section, you will see that the [TF-IDF](https://monkeylearn.com/blog/what-is-tf-idf/) for each word in each patent was calculated. This was done in case a future version included an improved comparison which accounted for not only the words, but how unique and important they were.

<h3>Final State</h3>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <p>Unfortunately, it is difficult to show a proper demo for this tool - the Lens API key has expired, and the NIH Federal Reporter API was retired.</p>
        <p>However, I have some screenshots and past downloads to give some idea of what it looked like while functional. </p>
        <p>Following the instructions on the Github project page would display the page shown on the right:</p>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/ribio.png" title="search tool screenshot" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

The user would primarily be interacting with the "keyword" section. They could filter results by the author, location, etc. Where applicable, these filters would be inserted into the API request. 

The Lens ID was for convenience - if a user had a specific Lens ID (a specific scholarly article/patent) they wanted to look up within this tool, they could do so.

After entering different parts of the query, the user could choose to see an author analysis ("Authors") of the query or a patent analysis ("Claims"). The two analyses were separated because the patent analysis was significantly slower, so the user didn't need to wait for it unless they needed to.

Click below to see the results of looking up certain terms in the tool.

- <a href="{{ '6115_final_report.pdf' | prepend: 'assets/pdf/' | relative_url}}" target="_blank" rel="noopener noreferrer">Nitinol Paddle Lead (no filters)</a>
  - This term yielded no results from the Clinical Trials database or the NIH database. 
  - This was a "Claims" search, and the analyses tab displays similar patents. Index1/Index2 in the Claims tab map to patent's index in the lens_p_results tab, so it is easy to get more information.

<h3>Links</h3>
[Full source code](https://github.com/jasmine2000/ri-bio-project)