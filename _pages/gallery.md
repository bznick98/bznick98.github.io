---
layout: default
title: Gallery
description: Gallery Page
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

		{% for post in site.posts reversed %}

		<div class="portfolio-item">

			<a class="portfolio-item__link" href="{{ post.url | relative_url }}">

				<div class="portfolio-item__image">
					<img src="{{ post.featured_image | relative_url }}" alt="{{ post.title }}">
				</div>

				<div class="portfolio-item__content">
					<div class="portfolio-item__info">
						<h2 class="portfolio-item__title">{{ post.title }}</h2>
						<p class="portfolio-item__subtitle">{{ post.subtitle }}</p>
					</div>
				</div>

			</a>

		</div>

		{% endfor %}

	</div>

</section>