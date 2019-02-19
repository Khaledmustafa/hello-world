#!/usr/bin/env ruby
# The above tell the terminal it is Ruby
# Do not forget to chmod +x the file in terminal

require 'shopify_api' # We need to use the api!

module ShopifyTest # encapsulates the methods and constants
# the self. in the methods refers to this module (ShopifyTest)

  # Fill these out from your store (https://storename.myshopify.com/admin/apps/private)
  API_KEY=""
  PASSWORD=""
  SHOP_NAME="teachme-test"

 # Initialize and start session for private app
 # See Getting Started Here: https://github.com/Shopify/shopify_api
  def self.init
    shop_url = "https://#{API_KEY}:#{PASSWORD}@#{SHOP_NAME}.myshopify.com/admin"
    ShopifyAPI::Base.site = shop_url
  end

  def self.delete_all_products
    count = ShopifyAPI::Product.count # Get a count of all products on a shop
    beans = 0 # number of products we have deleted

    while beans < count # while we have deleted less than the total number of products ...
      products = ShopifyAPI::Product.find(:all, :params => {:limit => 250}) # Get a list of 250 words
      products.each do |product| # loop through each product in that list
        ShopifyAPI::Product.delete(product.id) # Delete product
        print "Deleted #{beans} of #{count}| Product Title -> #{product.title} \r" # Output that
        beans = beans + 1 # We have deleted a product, increment counter
        sleep 0.5 # Sleep so to not go over Shopify's rate
      end
    end

  end

  # Words to use to create products
  WORDS = ["angle", "ant", "apple", "arch", "arm", "army", "baby", "bag", "ball", "band", "basin", "basket", "bath", "bed", "bee", "bell", "berry", "bird", "blade", "board", "boat", "bone", "book", "boot", "bottle", "box", "boy", "brain", "brake", "branch", "brick", "bridge", "brush", "bucket", "bulb", "button", "cake", "camera", "card", "cart", "carriage", "cat", "chain", "cheese", "chest", "chin", "church", "circle", "clock", "cloud", "coat", "collar", "comb", "cord", "cow", "cup", "curtain", "cushion", "dog", "door", "drain", "drawer", "dress", "drop", "ear", "egg", "engine", "eye", "face", "farm", "feather", "finger", "fish", "flag", "floor", "fly", "foot", "fork", "fowl", "frame", "garden", "girl", "glove", "goat", "gun", "hair", "hammer", "hand", "hat", "head", "heart", "hook", "horn", "horse", "hospital", "house", "island", "jewel", "kettle", "key", "knee", "knife", "knot", "leaf", "leg", "library", "line", "lip", "lock", "map", "match", "monkey", "moon", "mouth", "muscle", "nail", "neck", "needle", "nerve", "net", "nose", "nut", "office", "orange", "oven", "parcel", "pen", "pencil", "picture", "pig", "pin", "pipe", "plane", "plate", "plow", "pocket", "pot", "potato", "prison", "pump", "rail", "rat", "receipt", "ring", "rod", "roof", "root", "sail", "school", "scissors", "screw", "seed", "sheep", "shelf", "ship", "shirt", "shoe", "skin", "skirt", "snake", "sock", "spade", "sponge", "spoon", "spring", "square", "stamp", "star", "station", "stem", "stick", "stocking", "stomach", "store", "street", "sun", "table", "tail", "thread", "throat", "thumb", "ticket", "toe", "tongue", "tooth", "town", "train", "tray", "tree", "trousers", "umbrella", "wall", "watch", "wheel", "whip", "whistle", "window", "wing", "wire", "worm"]
  ACTION_WORDS = ["Active",  "Exquisite",  "Pretty",  "Awkward",  "Hideous",  "Ugly",  "Adept",  "Fair",  "Ravishing",  "Bizarre",  "Homely",  "Ungainly",  "Adroit",  "Fascinating",  "Robust",  "Cadaverous",  "Horrible",  "Unkempt",  "Agile",  "Good-looking",  "Shapely",  "Clumsy",  "Incongruous",  "Unmanly",  "Attractive",  "Graceful",  "Skillful",  "Coarse",  "Invidious",  "Unwomanly",  "Beautiful",  "Handsome",  "Spirited",  "Decrepit",  "Loathsome",  "Weak",  "Brawny",  "Hardy",  "Spruce",  "Effeminate",  "Odious",   "Charming",  "Immaculate",  "Stalwart",  "Emaciated",  "Repellent",   "Comely",  "Lively",  "Strapping",  "Feeble",  "Repugnant",   "Dainty",  "Lovely",  "Strong",  "Frail",  "Repulsive",   "Dapper",  "Manly",  "Sturdy",  "Gawky",  "Sickly",   "Delicate",  "Muscular",  "Virile",  "Ghastly",  "Slovenly",   "Dexterous",  "Neat",  "Vivacious",  "Graceless",  "Spare",   "Elegant",  "Nimble",  "Winsome",  "Grotesque",]

  def self.create_products
    count = 20
    for i in 1..count # loop through 1 to count
      @name = ACTION_WORDS.sample + " " + WORDS.sample.capitalize # get a random word from action words, and random word from words... capitalize them and append them together
      product = ShopifyAPI::Product.create(title: @name, product_type: 'test', vendor: 'test') # Create a product
      puts "Created #{i} of #{count} with name #{product.title} \r" # Output this
      sleep 0.5 #Sleep so you don't go over the rate limit 
    end
  end

end

# Gets the arguments, and then either creates or deletes
#  ./file_name.rb create
#  ./file_name.rb delete
ARGV.each do|arg|
  ShopifyTest.init # Always init
  if arg == 'create' # If create, run create_products
    ShopifyTest.create_products
  elsif arg == 'delete' # Otherwise if delete, run delete_products
    ShopifyTest.delete_all_products
  end
end
