This is a basic voting gem that can be added to any model that you wish to be subject to votes. 

To Initialize this gem a polymorphic association is needed so in the database a votes table is required unique to and customizable to your needs. For example: 

  create_table "votes", force: true do |t|
    t.boolean  "vote"
    t.integer  "user_id"
    t.integer  "voteable_id"
    t.string   "voteable_type"
    t.datetime "created_at"
    t.datetime "updated_at"
  end
	
	Then a vote model is also required again customizable to your needs
	As an example with the above database our vote model would be:
	
	class Vote < ActiveRecord::Base
	  belongs_to :creator, class_name: 'User', foreign_key: 'user_id'
	  belongs_to :voteable, polymorphic: true

	  validates_uniqueness_of :creator, scope: [:voteable_id, :voteable_type]
	end
	
	Validation can be added as necessary the simple validation above allows only one
	vote per user.
	
	Finally with the ez_votes gem installed all you must do is add
	include Voteable in the model you wish to be subject to votes