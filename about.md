---
title: About
layout: page
---
![Profile Image]({% if site.external-image %}{{ site.picture }}{% else %}{{ site.url }}/{{ site.picture }}{% endif %})

<p>Lorem Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>

<p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>

<h2>Skills</h2>

<ul class="skill-list">
	<li>HTML - Jade - Haml - Erb</li>
	<li>Responsive (Mobile First)</li>
	<li>CSS (Stylus, Sass, Less)</li>
	<li>Css Frameworks (Bootstrap, Foundation)</li>
	<li>Javascript (Design Patterns, Tests)</li>
	<li>AngularJS - ReactJS</li>
	<li>Grunt - Gulp - Yeoman</li>
	<li>Git</li>
	<li>PHP</li>
	<li>Python</li>
	<li>MySQL - MongoDB</li>
	<li>Scrum and Kanban</li>
	<li>TDD e Continuous Integration</li>
</ul>

<h2>Projects</h2>

<ul>
	<li><a href="https://github.com/">Lorem Lorem</a></li>
	<li><a href="https://github.com/">Ipsum Dolor</a></li>
	<li><a href="https://github.com/">Dolor Lorem</a></li>
</ul>

<h2>Publications</h2>

<div id="publications">Loading publications...</div>

<script>
(async function() {
  const authorId = '{{ site.semanticscholar }}';
  if (!authorId) {
    document.getElementById('publications').innerHTML = '<p>No Semantic Scholar ID configured.</p>';
    return;
  }
  
  try {
    const response = await fetch(
      `https://api.semanticscholar.org/graph/v1/author/${authorId}/papers?fields=title,year,venue,authors,externalIds,url&limit=50`
    );
    const data = await response.json();
    
    if (!data.data || data.data.length === 0) {
      document.getElementById('publications').innerHTML = '<p>No publications found.</p>';
      return;
    }
    
    // Sort by year descending
    const papers = data.data.sort((a, b) => (b.year || 0) - (a.year || 0));
    
    let html = '<ul class="publications">';
    for (const paper of papers) {
      const authors = paper.authors ? paper.authors.map(a => a.name).join(', ') : '';
      const venue = paper.venue || '';
      const year = paper.year || '';
      const doi = paper.externalIds?.DOI || '';
      const arxiv = paper.externalIds?.ArXiv || '';
      
      html += `<li>
        <strong><a href="${paper.url}" target="_blank">${paper.title}</a></strong><br>
        ${authors}<br>
        <em>${venue}</em>${venue && year ? ', ' : ''}${year}`;
      if (doi) html += ` | <a href="https://doi.org/${doi}" target="_blank">DOI</a>`;
      if (arxiv) html += ` | <a href="https://arxiv.org/abs/${arxiv}" target="_blank">arXiv</a>`;
      html += `</li>`;
    }
    html += '</ul>';
    
    document.getElementById('publications').innerHTML = html;
  } catch (error) {
    console.error('Failed to fetch publications:', error);
    document.getElementById('publications').innerHTML = '<p>Failed to load publications.</p>';
  }
})();
</script>
