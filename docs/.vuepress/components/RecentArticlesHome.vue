<template>
<div class="features">
  <div class="feature" v-for="content in contents" v-bind:key="content.id">
  <h3><code><a :href="`/${content}/`" class="nav-link action-button">{{ content.split('-')[1].replace("%20"," ") }}</a></code></h3>
  <div v-if="recentFilesDic[content].length > 0">
	<ul>
		<li v-for="post in recentFilesDic[content]" v-bind:key="post.id">
      <a :href="post.path">{{post.title}}</a>
			<p v-if="post.frontmatter.meta" style="font-size:0.8em; margin-top:0.1em; margin-bottom:0.3em;">: {{post.frontmatter.meta[0].content}}</p>
		</li>
	</ul>
  </div>
  <div v-else>
    <p>[준비중]</p>
  </div>
  </div>
</div>
</template>

<script>

export default {
	data() {
		return {
      contents: ['01-Infrastructure', '02-Private%20Platform', '03-Public%20Cloud', '04-HashiCorp', '05-Software', '06-etc'],
      recentFilesDic: {}
    };
	}, 
	created() {
    this.contents.forEach(content => {
      let files = this.$site.pages.filter(p => {
				return p.path.indexOf(`/${content}`) >= 0;
			}).sort((a,b) => {
				let aDate = new Date(a.lastUpdated).getTime();
				let bDate = new Date(b.lastUpdated).getTime();
				let diff = aDate - bDate;
				if(diff > 0) return -1;
				if(diff < 0) return 1;
				return 0;
			}).slice(0,4);

      this.recentFilesDic[content] = files
			// return [files.slice(0,7), files.slice(7,14)];
    })
    // console.log(this.recentFilesDic)
  }
}

</script>