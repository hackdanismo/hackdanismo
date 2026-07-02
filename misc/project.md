# Web Project
A document that outlines a possible web project.

+ [Content Management System](#content-management-system)
    + [Headless CMS](#headless-cms)
+ [Frontend](#frontend)

## Content Management System
A `CMS`, or `Content Management System`, is software that helps users to `create`, `edit`, `organize`, and `publish` content, such as web pages, without needing to write code for every change. Examples of `Content Management Systems` include: 

+ [WordPress](https://en-gb.wordpress.org/)
+ [Drupal](https://new.drupal.org/home)
+ [Contentful](https://www.contentful.com)
+ [Storyblok](https://www.storyblok.com)
+ [Sanity](https://www.sanity.io)
+ [Strapi](https://strapi.io/)
+ [django CMS](https://www.django-cms.org/en/)
+ [Decamp](https://decapcms.org/)
+ [Nuxt Content](https://content.nuxt.com/)

### Headless CMS
A `headless CMS` is a `Content Management System` where the content backend is separated from the frontend presentation layer. In a traditional `CMS`, like `WordPress` in its classic setup, the `CMS` often manages both the content and frontend. A headless CMS only manages the content. It exposes that content through an `API`, usually `REST` or `GraphQL`, so it can be displayed anywhere: a website, mobile app, kiosk, smart TV app, email, or other digital product.

Headless `CMS` examples:

+ `Contentful` - popular SaaS headless CMS, common in enterprise and marketing sites.
+ `Sanity` - very flexible content modeling, strong developer experience.
+ `Strapi` - open-source, self-hostable or cloud-hosted.
+ `Storyblok`	- headless CMS with a visual editor.
+ `Prismic`	- headless CMS focused on page sections/slices and marketing sites.
+ `Hygraph`	- GraphQL-first headless CMS.
+ `DatoCMS`	- SaaS headless CMS, popular with static-site and Jamstack builds.
+ `Directus` - API/data-platform style CMS over SQL databases.
+ `Payload CMS`	- developer-focused, TypeScript-based, often used with Next.js.
+ `Kontent.ai` - enterprise headless CMS.

Traditional `CMS` that can be used headless:

+ `WordPress`	- can be used headlessly via the REST API or GraphQL plugins.
+ `Drupal` - supports "decoupled" or headless Drupal, where Drupal acts as a content API.
+ `Craft CMS`	- can be used headlessly with APIs/GraphQL.
+ `Umbraco`	- has headless/Content Delivery API options.
+ `Adobe Experience Manager` - enterprise CMS with headless content delivery features.
+ `Sitecore` - enterprise CMS/DXP with headless capabilities.

## Frontend
For a SaaS website, the best frontend for working with a CMS is usually `Next.js`. It is the safest default because it handles the main SaaS website needs well: `SEO`, `fast landing pages`, `blog/content pages`, `dynamic routes`, `preview mode`, `analytics scripts`, `forms`, `A/B testing`, and easy deployment. It also works well with most headless CMSs and is especially common with: `Sanity`, `Contentful`, `Storyblok`, `Prismic`, `Strapi`, `Payload`, `Hygraph`, and `WordPress headless`.

Choose `Next.js` when your SaaS site has:

+ Marketing pages
+ Blog or resources section
+ Case studies
+ SEO landing pages
+ Dynamic pages from a CMS
+ Product-led signup flows
+ A React-based team or app
