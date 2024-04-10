<script lang="ts">
    export let onMobile: boolean;
</script>
<footer class="py-6">
	<div class="container flex flex-col items-center justify-center gap-4 md:h-24 md:flex-row">
		<div class="flex flex-col items-center gap-1 md:flex-row px-4">
			<p class="text-center text-sm leading-loose text-muted-foreground md:text-left">
				Developed by
                <a
					href="https://github.com/syslev"
					target="_blank"
					rel="noreferrer"
					class="font-medium underline underline-offset-4"
				>
					lev</a
				>.
			</p>
            <p class="text-center text-sm leading-loose text-muted-foreground md:text-left">
                Source available on
                    <a
                        href="https://github.com/syslev/strava.tools/"
                        target="_blank"
                        rel="noreferrer"
                        class="font-medium underline underline-offset-4"
                    >
                        GitHub</a
                    >. Licensed under <a
                    href="https://github.com/syslev/strava.tools/blob/main/LICENSE"
                    target="_blank"
                    rel="noreferrer"
                    class="font-medium underline underline-offset-4"
                >GPL-3.0</a>
            </p>
            <img src="img/icon/powered_by_strava.svg" class="h-4 inline p-0" title="Powered by Strava" alt="Powered by Strava">        
		</div>
	</div>
</footer>