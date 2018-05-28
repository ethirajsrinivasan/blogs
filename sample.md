<h1>This is a main heading</h1>
<p>sample</p>

```ruby
gem install rails
```

```ruby
class Blog < ApplicationRecord
  extend FriendlyId
  friendly_id :title, use: [:slugged, :finders]

  validates :title,        presence: true
  validates :content_url,  presence: true
  validates :state,        inclusion: { in: ['draft', 'published'] }
  validates :published_at, presence: true, if: :published?

  

  #paginates_per 8

  def published?
    state == 'published'
  end

  #
  # Fetches the content from the github
  #
  def content
    Octokit.contents("ethirajsrinivasan/blogs",
                     path: content_url,
                     accept: 'application/vnd.github.v3.html')
  end
end
```
