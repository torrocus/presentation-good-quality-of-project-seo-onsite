## Good quality of project
### SEO Onsite

by

_Alex M._, [Web developer](http://fractalsoft.org/en/team/torrocus "Ruby on Rails developer") at Fractal Soft

Note: Why this title? Case study



### Developer responsibility

- Implementation of new features <!-- .element: class="fragment" data-fragment-index="1" -->
- Application maintenance (updates & upgrades) <!-- .element: class="fragment" data-fragment-index="2" -->
- Create tests and bug fixes <!-- .element: class="fragment" data-fragment-index="3" -->


### Developer responsibility
#### In my opinion

1. Help the product owner (understand a problem & propose a solution). <!-- .element: class="fragment" data-fragment-index="1" -->
2. Help users get product they are looking for. <!-- .element: class="fragment" data-fragment-index="2" -->
3. Create good product (implementation, testing, maintenance). <!-- .element: class="fragment" data-fragment-index="3" -->
4. Help you get familiar users with the project and product owner. <!-- .element: class="fragment" data-fragment-index="4" -->




### SEO
#### Help Google find you
##### Mistakes that affect the product



### Mistake with robots.txt

File robots.txt block all robots
```
User-agent: *
Disallow: /
```


### Mistake with robots.txt

Solutions:
- remove robots.txt
- change robots.txt
```
User-agent: *
Allow: /
```


### Mistake with robots.txt

OK, but I have:
- production
- staging
- develop

Do not index staging & develop in Google!


### Mistake with robots.txt

Creating a dynamic robots.txt
```
# routes.rb
get '/robots.:format' => 'pages#robots'

# app/controllers/pages_controller.rb
def robots
  respond_to :text
  expires_in 6.hours, public: true
end

# app/views/pages/robots.text.erb
<% if Rails.env.production? %>
  User-Agent: *
  Allow: /
<% else %>
  User-Agent: *
  Disallow: /
<% end %>
```

Note: Doesn't work if app is down


### Mistake with robots.txt

Change nginx configuration in staging:
```
location /robots.txt {
  alias /staging-app/public/robots.staging.txt;
}
```



### Mistake with title

Empty or static title
```
<title>My App</title>
```

Note: Google use title as a anchor


### Mistake with title

Rules for creating title:
- title talks about content <!-- .element: class="fragment" data-fragment-index="1" -->
- first few words are the most important <!-- .element: class="fragment" data-fragment-index="2" -->
- do not duplicate the same text from H1 tag <!-- .element: class="fragment" data-fragment-index="3" -->
- no more than 70 characters, including spaces <!-- .element: class="fragment" data-fragment-index="4" -->
- do not duplicate titles between pages <!-- .element: class="fragment" data-fragment-index="5" -->

Note: Avoid using stop words like: the, an, and, but, if, which, we


### Mistake with title

Creating a dynamic title if it is possible
```
# app/views/layouts/application.html.erb
<title>
  <%= yield(:title).presence || 'My App' %>
</title>

```


### Mistake with title

Or even better, use translations
```
# app/views/layouts/application.html.erb
<title>
  <%= yield(:title).presence || t(:title, scope: :head) %>
</title>

```



### Mistake with meta description

Empty or static description
```
<meta content="" name="description" />
```

[SRUG](http://www.lmfgtfy.com/?q=srug)

Note: Google gets the content if no description and uses first sentences. Search engines won't always use meta description.


### Mistake with meta description

Rules for creating description:
- use the most important keywords <!-- .element: class="fragment" data-fragment-index="1" -->
- write for people, not for search engines <!-- .element: class="fragment" data-fragment-index="2" -->
- no more than 160 characters, including spaces <!-- .element: class="fragment" data-fragment-index="3" -->
- do not duplicate meta descriptions between pages <!-- .element: class="fragment" data-fragment-index="4" -->


### Mistake with meta description

```
<meta
  name='description'
  content='<%= yield(:description).presence ||
           t(:description, scope: :head)} %>'
>
```



### Mistake with h1 tag

- Many tags on one page. <!-- .element: class="fragment" data-fragment-index="1" -->
- h1 tag used for trivial content or styling. <!-- .element: class="fragment" data-fragment-index="2" -->



### Mistake with translations

- Page language is not specified.
- Another language in meta and on page.


### Mistake with translations

Determine what language you use:
```
# app/views/layouts/application.html.erb
<head lang='<%= I18n.locale %>'>
```


### Mistake with translations

I18n in Rails
```
# config/locales/en.yml
en:
  logo: Logo
  users:
    index:
      header: List of users
      subheader_html: "In <strong>random</strong> order"

# app/views/users/index.html.erb
<h1><%= t('.header') %></h1>
<h2><%= t('.subheader') %></h2>
<img src='...' alt='<%= t(:logo) %>'>
```

[Rails I18n](http://guides.rubyonrails.org/i18n.html)


### Mistake with translations

Use I18n with Active Record
```
# config/locales/en.yml
en:
  activerecord:
    models:
      user: User
    attributes:
      user:
        email: Email
        password: First name
```


### Mistake with translations

And use I18n even with Simple Form
```
# config/locales/en.yml
en:
  simple_form:
    hints:
      user:
        email: Note that the email address must contain @
    placeholders:
      user:
        password: Put your secret password here
    prompts:
```



### Mistake with links

Types of links:
- internal (good for SEO)
- external (bad for SEO)


### Mistake with links

Internal links
```
<%= link_to t(:application_name), root_path %>
# => <a href="/">My App</a>
```

Always should be dofollow. <!-- .element: class="fragment" data-fragment-index="1" -->


### Mistake with links

Add title for internal links (for users & SEO)
```
<%= link_to(
      t(:name, scope: :app),
      root_path,
      title: t(:title, scope: :app)
    )
%>
# => <a href="/" title="Great App">My App</a>
```


### Mistake with links

External links
```
<%= link_to 'Google', 'http://google.com/' %>
# => <a href='http://google.com/'>Google</a>
```

Should this link be nofollow? <!-- .element: class="fragment" data-fragment-index="1" -->


### Mistake with links

External links with nofollow (better for SEO)
```
<%= link_to 'Google', 'http://google.com/', rel: 'nofollow' %>
# => <a rel="nofollow" href="http://google.com/">Google</a>
```


### Mistake with links

Two links with different anchor, but the same target.
```
# views
<%= link_to 'First link', root_path %>
<%= link_to 'Second link', root_path %>
```

Google index only first link. <!-- .element: class="fragment" data-fragment-index="1" -->


### Mistake with links

Priorities for links:
- link with image <!-- .element: class="fragment" data-fragment-index="1" -->
- link with title <!-- .element: class="fragment" data-fragment-index="2" -->
- link <!-- .element: class="fragment" data-fragment-index="3" -->
- link with nofollow <!-- .element: class="fragment" data-fragment-index="4" -->




### Mistake with layouts

- One layouts with many if-conditions. <!-- .element: class="fragment" data-fragment-index="1" -->
- Long page loading (many or big js files) <!-- .element: class="fragment" data-fragment-index="2" -->


### Mistake with layouts

Solution:
- Create separate layout for not logged in users (public.html.erb).
- Do not load frontend applications for non-logged users.

Google likes quick pages!



### Mistake with social media

Application does not have meta tags for social media.


### Mistake with social media

Use Open Graph protocol for public pages.

Even the simplest version:
```
<meta name='twitter:card' content=SUMMARY />
<meta property='og:title' content=TITLE />
<meta property='og:type' content=TYPE />
<meta property='og:url' content=URL />
<meta property='og:image' content=IMAGE_URL />
<meta property='og:description' content=DESCRIPTION />
```

[Open Graph protocol](http://ogp.me/)



### Helpful tools


### MetaTags

Make your Rails application SEO-friendly
```
# Gemfile
gem 'meta-tags'
```

[MetaTags](https://github.com/kpumuk/meta-tags)


### MetaTags

```
# app/views/layouts/application.html.erb
<head lang='<%= I18n.locale %>'>
  <meta charset='utf-8'>
  <%= display_meta_tags(default_meta_tags) %>
</head>

# app/helpers/application_helper.rb
def default_meta_tags
  I18n.t(:meta).merge(separator: '-')
end

# config/locales/en.yml
en:
  meta:
    description: DESCRIPTION
    keywords: KEYWORDS
    site: SITE NAME
    title: TITLE
```


### Route Translator

Translates the Rails routes using locale files
```
# Gemfile
gem 'route_translator'
```

[Route Translator](https://github.com/enriclluelles/route_translator)


### Route Translator

```
# config/routes.rb
Rails.application.routes.draw do
  localized do
    resources :users, only: :index
  end
end

# config/locales/en.yml
en:
  routes:
    users: users

# config/locales/fr.yml
fr:
  routes:
    users: utilisateurs
```


### Route Translator
```
$ rake routes
  Prefix Verb URI Pattern                Controller#Action
users_fr GET  /fr/utilisateurs(.:format) users#index {:locale=>"fr"}
users_en GET  /users(.:format)           users#index {:locale=>"en"}
```



## Questions?



## Thank you!



## Contact

@torrocus
