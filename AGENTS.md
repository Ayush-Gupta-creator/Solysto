<!-- LOVABLE:BEGIN -->
> [!IMPORTANT]
> This project is connected to [Lovable](https://lovable.dev). Avoid rewriting
> published git history — force pushing, or rebasing/amending/squashing commits
> that are already pushed — as it rewrites history on Lovable's side and the
> user will likely lose their project history.
>
> Commits you push to the connected branch sync back to Lovable and show up in
> the editor, so keep the branch in a working state.
<!-- LOVABLE:END -->

## Website architecture
- Keep the product catalogue in `src/data/products.ts` and reuse it across listing, detail and enquiry views so descriptions and product references remain consistent.
- Keep scroll-controlled hero media in `src/components/scroll-hero.tsx`, using local `public/media/` video/poster files and reduced-motion fallback so the client's film remains replaceable.
- Keep shared navigation, footer and repeated catalogue presentation in `src/components/site.tsx`; each public content route owns its page metadata.
- Serve all media from local files in `public/media/` (referenced as `/media/<file>`) so the site can be hosted outside Lovable; keep `src/assets/*.asset.json` pointers only as source records. Preserve the original product appearance when preparing presentation crops.
- Match hero progress to the full-height sticky viewport; seek only on demand and coalesce pending seeks to the latest target, using direct mobile seeks rather than a perpetual animation loop.

- Listen for live reduced-motion preference changes; use native manually operated hero video controls without scroll listeners in calm mode, and retain the poster until media is ready.

- Choose a codec-supported mobile or desktop hero asset after hydration; fetch it immediately alongside the lightweight poster and automatically choose the smaller video on slow/data-saving connections, so poster readiness cannot delay loading.
- Use scroll seeking in normal motion mode and native manual playback only in reduced-motion mode, so playback never competes with seeks.
- Key product-photo reveal animations by product slug so catalogue and related-product navigation both animate the selected original photo.
- Centralize site-wide reveals in `src/components/site-motion.tsx` with one intersection observer, dynamic-content registration and live reduced-motion cleanup; keep these observers separate from hero video seeking to avoid playback contention.
- Keep Instagram promotion in one shared accessible dialog with session-only dismissal and a manual opener, so route changes do not repeat the popup.
- Prepare dark catalogue photos with background masks and original source pixels rather than generated replacements, so product details are not invented; retain original asset pointers.

- Keep optional collection-only photo variants in the shared catalogue and opt into them from the collection route, so detail and related-product photos remain unchanged.
