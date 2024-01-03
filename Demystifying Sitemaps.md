### Demystifying Sitemaps: Your Pathway to Seamless Website Exploration
In the vast world of the internet, navigating a website can sometimes be hard. That's where the sitemap comes into the picture. Sitemap is an essential tool to help visitors navigate your website easily. It also helps search engines to help index your websites efficiently. In this blog post, we will look in detail at sitemaps, their benefits, formats, and a practical example of how to generate them.

![path](https://images.unsplash.com/photo-1472740378865-80aab8e73251?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2940&q=80)

#### What is a Sitemap?
A sitemap contains a list of all the pages of a website organized in a hierarchical manner. It helps visitors and search engine crawling robots understand a website's layout and structure. A sitemap not only contains the content pages but also includes metadata like priority, last modified, and change frequency of the page.

#### Benefits of Using Sitemaps
* Improved Indexing: Search engines like Google use sitemap to efficiently index the content on the website. Once the content on the web is indexed it can appear in search results.
* Better User Experience: Sitemap enhances user experience by helping users to locate the desired page. It also helps reduce bounce rates.
* Discovering Hidden Pages: Sitemap helps locate hidden pages that are not easily discoverable. It also helps index those pages and increases visibility in search results.
* SEO Boost: Sitemap boosts SEO performance and helps in higher ranking in search engine results.

#### Formats of Sitemap
Sitemap is supported in different formats with each format for a specific purpose. The most commonly used formats are as follows:
* XML Sitemap: This is the most common format of sitemps. It is an XML structured format with the URL of the website pages along with metadata like priority, last modified time, and frequency change. This format is used by search engines like Google, Bing, and Yahoo to index the website's pages. Each URL can also include optional information like images, videos, and mobile-specific.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://www.example.com/page1</loc>
    <lastmod>2023-08-25</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.8</priority>
  </url>
  <url>
    <loc>https://www.example.com/page2</loc>
    <lastmod>2023-08-20</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.6</priority>
  </url>
  <!-- More URLs here -->
</urlset>
```

* HTML Sitemap: HTML sitemaps are mainly generated for human use. An HTML Sitemap is a webpage containing links to all the pages of the website. These links are organized hierarchically or categorically for easy navigation to the human user.
```xml
<!DOCTYPE html>
<html>
<head>
  <title>HTML Sitemap</title>
</head>
<body>
  <h1>Website Navigation</h1>
  <ul>
    <li><a href="/page1">Page 1</a></li>
    <li><a href="/page2">Page 2</a></li>
    <!-- More list items for other pages -->
  </ul>
</body>
</html>
```

* Text Sitemap: A text sitemap is a text file containing the list of URLs of the website. It assists search engines to discover the content of the website. It is a simpler version of the XML type which smaller websites can use.

```xml
https://www.example.com/page1
https://www.example.com/page2
```

* Image Sitemap: An image sitemap is an extension of the XML sitemap focusing on images. It provides specific information about the images on the website like location, title, caption, license, and other relevant data. It helps search engines index and display images in image search results.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
        xmlns:image="http://www.google.com/schemas/sitemap-image/1.1">
  <url>
    <loc>https://www.example.com/image1</loc>
    <image:image>
      <image:loc>https://www.example.com/image1.jpg</image:loc>
    </image:image>
  </url>
  <url>
    <loc>https://www.example.com/image2</loc>
    <image:image>
      <image:loc>https://www.example.com/image2.jpg</image:loc>
    </image:image>
  </url>
  <!-- More URLs with images -->
</urlset>
```

* Video Sitemap: Similar to image sitemap, video sitemap focuses on videos. It provides information like title, description, play page URL, and thumbnail images. It helps in better search engine results of your videos.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9"
        xmlns:video="http://www.google.com/schemas/sitemap-video/1.1">
  <url>
    <loc>https://www.example.com/video1</loc>
    <video:video>
      <video:thumbnail_loc>https://www.example.com/video1-thumbnail.jpg</video:thumbnail_loc>
      <video:title>Video 1 Title</video:title>
      <video:description>This is the description for Video 1.</video:description>
      <video:content_loc>https://www.example.com/video1.mp4</video:content_loc>
    </video:video>
  </url>
  <url>
    <loc>https://www.example.com/video2</loc>
    <video:video>
      <!-- Video 2 details here -->
    </video:video>
  </url>
  <!-- More URLs with videos -->
</urlset>
```

Now that we have seen the benefits and formats we shall look into an example.

#### Creating a Sitemap in Ruby on Rails
In this example, we will use `sitemap_generator` to create a sitemap in ruby on rails
* Install:
Add the `sitemap_generator` gem to your Gemfile:
```ruby
gem 'sitemap_generator'
```
bundle install to install the gem.

* Generate Configuration

```ruby
rails generate sitemap:install
```

This will generate a ruby file  `config/sitemap.rb` which you can use to configure sitemap settings.

* Configure Sitemap

Modify the sitemap.rb as per your application. You can define URLs, set priority, update frequency, and add the last modified time.

```ruby
SitemapGenerator::Sitemap.create do
  add root_path, priority: 1.0, changefreq: 'daily'

  Post.find_each do |post|
    add post_path(post), lastmod: post.updated_at
  end
end
```
* Generate Sitemap:

```ruby
rake sitemap:refresh
```

XML Sitemap will be created in the `/public` directory and it will also ping the Google search engine to inform about the sitemap availability.
Do check the sitemap of my personal website at `https://ethigeek.com/sitemap.xml.gz`

#### Conclusion
Sitemaps are tools that help to solve the digital maze of the website. There are numerous sitemap tools available that support different languages and frameworks. So don't miss this important step in your website development process. Give visitors and search engines the clarity that is required.

