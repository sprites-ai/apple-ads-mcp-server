# Example prompts for the Apple Ads MCP server

These work as written in Claude, ChatGPT, Cursor, Codex, Gemini CLI and the other clients listed in the README. Anything that changes the account comes back as an approval first.

## Read and report

- With Sprites, how did Apple Ads do last 30 days vs the 30 before?
- Use Sprites to show Apple Ads search terms with spend and no installs this month.
- With Sprites, which Apple Ads keywords have the lowest cost per install?
- With Sprites, find new Apple Ads keywords for our app category.

## Change, create and launch

- Ask Sprites to add those search terms as negative keywords in Apple Ads.
- Have Sprites raise the Apple Ads bid on our brand keyword to $2.50.
- Use Sprites to set the Apple Ads campaign daily budget to $100.
- Ask Sprites to pause the Apple Ads ad groups with no installs in 30 days.
- Have Sprites review Apple's recommendations for our account and say which to apply.

## Recurring

- Use Sprites to send me a weekly Apple Ads install and CPI summary.

## Tips

- Name the campaign, ad set or page in full. Sprites receives only the request, not the earlier chat.
- Say the period ("last 30 days", "this quarter"); your client passes today's date so it resolves correctly.
- Working on several businesses? Say "switch to <business name>" first (`choose_business`).
- To compare channels, ask about them in one request, for example "compare Google and Meta CPA last month".
