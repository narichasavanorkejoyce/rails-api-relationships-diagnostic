# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
  Join tables hold references to two or more models. Join tables are valuable because they allow us to create relationships between these tables. Join tables let us have many-to-many relationships.
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
  ERD w table attributes: [Imgur](http://i.imgur.com/iA8N875.jpg)

  Profile:
  - id
  - given_name (string)
  - surname (string)
  - email (string)

  Movies:
  - id
  - title (string)
  - release_date (date)
  - length (integer)

  Favorites:
  - id
  - profile_id (indexed on profile(id))
  - movie_id (indexed on movie(id))
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
    has_many :movies, through: :favorites
    has_many :favorites

    validates :given_name, presence: true
    validates :surnamne, presence: true
    validates :email, presence: true
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
    has_many :profiles, through: :favorites
    has many :favorites
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
    belongs_to :profile, inverse_of: :favorites
    belongs_to :movie, inverse_of: :favorites
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
  A serializer converts data into a format that can be viewed on the browser. We need to add appropriate attributes to a serializer in order to see results displayed on a browser.
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes :id, :given_name, :surname, :email, :movies
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
  bin/rails generate scaffold favorites profile:references movie:references
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
  Use `dependent: destroy` when you want to delete an observation (for example, a movie) and the association attached to it. When we delete a movie, the reference to the profile will be deleted as well.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
  Musicians can have many teachers, teachers can have many musicians. We'd want to create this many-to-many relationship by using a join table called `school`. However, a musician may have a teacher that serves as an advisor. In that case, one advisor may have many advisee musicians, an advisee only has one advisor. We'd want to capture that association in a one-to-many relationship.
  ```
