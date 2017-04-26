# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
    # Additional columns and repeated data and storage not needed because access
    # to the data in other tables would be accessible via keys.
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
    # Profiles has many Movies through Favorites
    # Profiles has many Favorites
    # Movies belongs to many Profiles through Favorites (inverse_of)
    # Favorites has many Profiles
    # Profiles table has id, surname, given_name, email (string types)
    # Movies table has id, title, release_date, length (strings, dates, integer types)
    #
    # The Favorites join table would consist of a profile_id, movie_id, length if
    # desired
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
  has_many :movies, through: :favorites
  has_many :favorites, dependent: :destroy

  validates :given_name, presence: true
  validates :surname, presence: true
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
  has_many :profiles, through: :favorites
  has_many :favorites, dependent: :destroy

  validates :title, presence: true
  validates :release_date, presence: true
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
  belongs_to :profile, inverse_of: :favorites
  belongs_to :movies, inverse_of: :favorites
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
    # The serializer controls what fields are outputted to display e.g., below
    # would show in browser the first/last name of profile and list their favorites
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes :id, :given_name, :surname, :favorites
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
    # bin/rails generate scaffold favorites profile:references movies:references
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
    # "Dependent: Destroy" is a method that connects  the item as a dependent to
    # another object and it is used when a join table element/record is deleted and
    # then the relationship is destroyed between the tables.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
    # The cookbook lab is an example where the one-to-many relationship would be
    # useful as well as a many-to-many relationship because many ingredients belong
    # to one recipe but an ingredient can belong to many recipes, both through
    # recipe ingredients.  We went through this example yesterday
    # In the model, the has_many module and related methods would be needed for
    # the main tables as well as the join table. 
  ```
