<script>
    import "@onsvisual/svelte-components/css/main.css"; // This is more typically imported in the global +layout.svelte
    import {
        Hero,
        Highlight,
        Section,
        Grid,
        GridCell,
        Divider,
        Blockquote,
        Em,
        Scroller,
        ScrollerSection,
        Container,
    } from "@onsvisual/svelte-components";

    export let data; //import the data for the content

    const scrollerColors = ["#ddd", "#777", "#222"];
    let scrollerColor = scrollerColors[0];

    let contents = data.text.text.blocks;
    // console.log(contents);
</script>

{#each contents as block}
    {#if block.Type === "Hero"}
        <Hero
            theme="blue"
            title={block.Title}
            lede={block.Lede}
            date={block.Date}
            height={400}
        />
    {:else if block.Type === "Highlight"}
        <Highlight height={400} marginBottom={block.marginBottom === "true"}>
            {#each block.texts as text}
                <p>{text.value}</p>
            {/each}
        </Highlight>
    {:else if block.Type === "Section"}
        <Section marginTop={block.marginTop === "true"} title={block.Title}>
            {#each block.texts as text}
                {#if text.type === "text"}
                    <p>{@html text.value}</p>
                {:else if text.type === "Blockquote"}
                    <Blockquote attribution={text.value.Attribution}
                        >{@html text.value.Text}</Blockquote
                    >
                {/if}
            {/each}
        </Section>
    {:else if block.Type === "Grid"}
        <Grid cls="custom-grid" caption={block.Caption} rowHeight={200}>
            {#each block.cells as cell}
                <GridCell>
                    <div>{cell.value}</div>
                </GridCell>
            {/each}
        </Grid>
    {:else if block.Type === "Divider"}
        <Divider />
    {:else if block.Type === "Scroller"}
        <Scroller
            id={block.Id}
            splitscreen={block.Splitscreen === "true"}
            on:change={(e) => (scrollerColor = scrollerColors[e.detail.index])}
        >
            <div slot="background">
                <Container
                    width="full"
                    height="full"
                    background={scrollerColor}
                />
            </div>
            <div slot="foreground">
                {#each block.foreground.ScrollerSections as section}
                    <ScrollerSection>
                        <p>
                            {@html section.value}
                        </p>
                    </ScrollerSection>
                {/each}
            </div>
        </Scroller>
    {/if}
{/each}
