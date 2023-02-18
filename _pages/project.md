---
layout: default
title: Projects
description: Projects Page
featured_image: /images/gallery/duku.jpg
---

<section class="portfolio">
	<br><br>
	<div class="wrap">
		<h1>{{ page.title }}</h1>
		<br>
	</div>
</section>


<section class="portfolio">

	<div class="content-wrap wrap">

		{% for project in site.projects reversed %}

		<div class="portfolio-item">

			<a class="portfolio-item__link" href="{{ project.url | relative_url }}">

				<div class="portfolio-item__image">
					<img src="{{ project.featured_image | relative_url }}" alt="{{ project.title }}">
				</div>

				<div class="portfolio-item__content">
					<div class="portfolio-item__info">
						<h2 class="portfolio-item__title">{{ project.title }}</h2>
						<p class="portfolio-item__subtitle">{{ project.subtitle }}</p>
					</div>
				</div>

			</a>

		</div>

		{% endfor %}

	</div>

</section>