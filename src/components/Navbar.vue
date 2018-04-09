<template>
	<header class="navbar">
		<section class="navbar-section">
			<a href="#" class="btn btn-link logo"><img src="../assets/britecore-logo.png"/></a>
			<a href="#" class="btn btn-link mr-2"><i class="icon icon-bookmark"></i> Policies</a>
			<a href="#" class="btn btn-link mr-2"><i class="icon icon-flag"></i> Claims</a>
			<a href="#" class="btn btn-link mr-2"><i class="icon icon-people"></i> Contacts</a>
			<a href="#" class="btn btn-link mr-2"><i class="icon icon-flag"></i> Data</a>
			<a href="#" class="btn btn-link mr-2 active"><i class="icon icon-edit"></i> Lines</a>
			<a href="#" class="btn btn-link mr-2"><i class="icon icon-apps"></i> All Modules</a>
		</section>
	</header>
</template>
<script>
export default {
	name: 'Navbar'
}
</script>
<style lang="scss">
.navbar{ 
	padding: 5px;
	margin-bottom: 20px;

	.navbar-section{
		max-width: 95%;
		margin: 0 auto;
	}

	.logo{
		margin-right: 80px;
	}

	.btn.btn-link{
		color: rgba( 255,255,255, .8 );

		&:hover, &.active{
			background: rgba( 255,255,255,.3 )
		}
	}
}
</style>