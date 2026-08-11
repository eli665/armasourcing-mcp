# Tool reference

Live schemas come from the server itself:

```bash
curl -s -X POST https://chatgpt.armasourcing.com/mcp \
  -H 'Content-Type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

---

## `search_talent_pool`

`readOnlyHint: true`

Checks how well the vetted pool covers a specific brief. Returns match depth as a band, typical
experience, and the skills and tools that actually recur in matching candidates.

**It does not return candidates.** There are no names, no profiles and no ids. This is a
depth check, so an assistant can say "yes, there is real coverage for that" without exposing
real applicants or promising a specific person.

| Parameter | Type | Required | Notes |
| --- | --- | :---: | --- |
| `role` | string | yes | Role or job title, e.g. `executive assistant`, `bookkeeper`, `SEO specialist` |
| `skills` | string[] | no | Specific skills or tools, e.g. `["QuickBooks","Xero"]` |
| `timezone_overlap` | string | no | Business hours to cover, e.g. `9-5 EST` |

```bash
curl -s -X POST https://chatgpt.armasourcing.com/mcp \
  -H 'Content-Type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call",
       "params":{"name":"search_talent_pool",
                 "arguments":{"role":"bookkeeper","skills":["QuickBooks"]}}}'
```

---

## `list_supported_roles`

`readOnlyHint: true` · no parameters

The catalogue of roles and functions Armasourcing actually places, plus the managed contact
center offering. Use it to confirm a role is supported instead of guessing at one.

---

## `get_hiring_process`

`readOnlyHint: true` · no parameters

The end to end process with real timings: discovery call, matching in 3 to 5 business days, free
interviews, someone working in 5 to 7 days, and how replacements are handled.

---

## `get_engagement_terms`

`readOnlyHint: true` · no parameters

Contract terms, guarantees, oversight and security posture.

Armasourcing does not publish rates. This tool returns how pricing works and what is included,
and instructs the assistant not to estimate, range or guess an amount. If a user pushes for a
number, the correct answer is that it is quoted per role on the call.

---

## `get_booking_link`

`readOnlyHint: false` · `destructiveHint: false`

Returns the URL where the user books a free 15 minute discovery call, plus what to have ready
so the call is useful. Call it once the user is interested.

| Parameter | Type | Required | Notes |
| --- | --- | :---: | --- |
| `role` | string | no | Role they want to hire for |
| `hours_per_week` | string | no | e.g. `20` or `full time` |
| `timezone_overlap` | string | no | e.g. `9-5 PST` |
| `notes` | string | no | Context for the call |

**Do not put personal data in `notes`.** No names, emails, phone numbers or anything
identifying. The tool takes no personal details by design; the user enters those on the booking
form themselves. Optional context is logged anonymously so the team can see which roles are in
demand.
