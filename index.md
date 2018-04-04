---
---
# Projeto de Software / Computação / UFCG, 2018.1

> [Dalton Serey](http://daltonserey..github.io)<br>
> [Melina Mongiovi](http://www.dsc.ufcg.edu.br/~spg/Mongiovi/Home.html)<br>
> 
> Salas: ? e LCC3<br>
> Slack: [projsw-ufcg.slack.com](http://projsw-ufcg.slack.com)

{% assign curso = site.data %}

{% for licao in curso.licoes %}
<h3><a href="{{ licao.slides }}">{{ licao.titulo }}</a></h3>
{% endfor %}
