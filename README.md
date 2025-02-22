# web-ring
A GitHub managed webring pattern to connect personal websites in a Web1.0 &amp; Neocities style.

# Goals
- Webring functionality
- Functions standalone locally
- Define a pattern utilizing GitHub to manage a webring community in the style of an open source project

# Functionality
A member website will include a small javascript library with a \<script id="website id" source=...\> where the website ID is their chosen unique webring member ID. This script will use that ID to lookup their entry in the webring and reference the URL's belonging to their neighbors.

## Webring Structure
A circularly linked list where each element is indexed in a look-up table.

```
webring = {
  <id>: { url, previous, next },
  root: { url, previous=false, next },
  last: { url, previous, next=root }
}
```

## Add Website

```
addWebsite(webring/self, id, url) {
  if slug in webring return false

  previous = webring.last.previous
  website = { url, previous: previous, next: webring.root }
  previous.next = website
  webring.slug = website

  return true
}
```

## Remove Website

```
removeWebsite(webring/self, id) {
  if id not in webring return false

  toDelete = webring[id]
  previous = toDelete.previous
  next = toDelete.next

  previous.next = next
  next.previous = previous
  delete toDelete

  return true
}
```

## Update URL

```
updateURL(webring/self, id, newUrl) {
  if id not in webring return false
  webring[id].url = newUrl
  return true
}
```

## Insert Website
I'm not a fan of this feature in this context but the order in which members are set is arbitrary and an insert function will facilitate more complex reording should such behavior be desired.

## Usage Pattern
A single file containing the addition of each website in the web-ring to the linked list. A person would open a PR adding their website to the end of the list in an `addWebsite()` call. This file would be processed, thereby creating the web-ring data structure, which then needs output into a json lookup table where a user calling the client side JS library for the web-ring retrieves the links for the neighboring websites.

### Web-ring JSON Output

```
{
  "slug": { prevURL: "butts.com", nextURL: "nerd.com" },
  ...
}
```
