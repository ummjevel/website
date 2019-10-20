---
title: Flutter 위젯 목록
short-title: Widgets
show_breadcrumbs: false
---

{% assign sorted = site.data.catalog.widgets | sort:'name' -%}

Flutter에 포함된 대부분의 위젯이 알파벳 순서대로 작성되어있습니다.
또한 [카테고리별 위젯 목록][catalog]을 살펴볼 수 있습니다.

[Flutter YouTube 채널]({{site.social.youtube}})을 통해 Widget of the Week
영상 시리즈를 만나보실 수도 있습니다. 각양 각색의 위젯을 소개하는 짧은 에피소드들로 구성되어
있습니다. 더 많은 비디오를 [비디오](/docs/resources/videos)페이지에서 만나보세요.

<iframe width="560" height="315" src="https://www.youtube.com/embed/b_sQ9bMltGU" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
[Widget of the Week playlist](https://www.youtube.com/playlist?list=PLjxrf2q8roU23XGwz3Km7sQZFTdB996iG)

<div class="card-deck card-deck--responsive">
{% for comp in sorted %}
    <div class="card">
        <a href="{{comp.link}}">
            <div class="card-image-holder">
                {{comp.image}}
            </div>
        </a>
        <div class="card-body">
            <a href="{{comp.link}}"><header class="card-title">{{comp.name}}</header></a>
            <p class="card-text">{{comp.description}}</p>
        </div>
        <div class="card-footer card-footer--transparent">
            <a href="{{comp.link}}">Documentation</a>
        </div>
    </div>
{% endfor %}
</div>

[catalog]: /docs/development/ui/widgets
