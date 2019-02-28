---
layout: page
title: contact
permalink: /contact/
---
<form action="https://formspree.io/{{site.email}}" method="POST">
	<div class="row">
		<div class="col">
			<input type="hidden" name="_subject" value="Contact site">
			<input type="text" name="name" placeholder="Votre nom" required>
		</div>
		<div class="col">
			<input type="email" name="email" placeholder="Votre adresse mail" required>
		</div>
	</div>
	<div class="row">
		<div class="col">
			<textarea name="message" placeholder="Votre message" required ></textarea>
		</div>
	</div>
		<input type="text" name="_gotcha" style="display:none;" />
		<input type="hidden" name="_next" value="{{ page.url | prepend: site.baseurl }}" />
	<div class="row">
		<div class="col">
			<input type="submit" value="Envoyer" required>
		</div>
	</div>
</form>
