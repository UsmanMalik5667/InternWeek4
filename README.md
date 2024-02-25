class AuctionItem:
    def __init__(self, item_number, description, reserve_price):
        self.item_number = item_number
        self.description = description
        self.reserve_price = reserve_price
        self.num_bids = 0
        self.highest_bid = 0
        self.sold = False

# Task 1 - Auction set up
def setup_auction():
    auction_items = []

    while len(auction_items) < 6:
        print(f"\nAdding item {len(auction_items) + 1} to the auction:")
        item_number = input("Enter item number: ")
        description = input("Enter item description: ")

        while True:
            try:
                reserve_price = float(input("Enter reserve price: $"))
                if reserve_price < 0:
                    raise ValueError("Reserve price must be a non-negative number.")
                break
            except ValueError:
                print("Invalid input. Please enter a valid number for the reserve price.")

        auction_item = AuctionItem(item_number, description, reserve_price)
        auction_items.append(auction_item)
        
        print(f"Item {item_number} added to the auction successfully!\n")

    return auction_items

# Task 2 - Buyer bids
def place_bid(auction_items, buyer_number):
    item_number = input("Enter the item number you want to bid for: ")

    item = find_item(auction_items, item_number)

    if item is not None:
        print("\nItem Details:")
        print(f"Item Number: {item.item_number}, Description: {item.description}, "
              f"Current Highest Bid: ${item.highest_bid}")

        while True:
            try:
                new_bid = float(input("Enter your bid: $"))
                if new_bid <= item.highest_bid:
                    raise ValueError("Bid must be higher than the current highest bid.")
                break
            except ValueError:
                print("Invalid input. Please enter a valid number for the bid.")

        item.num_bids += 1
        item.highest_bid = new_bid
        print(f"\nBid placed successfully! New highest bid for Item {item.item_number}: ${item.highest_bid}")
    else:
        print(f"\nItem with number {item_number} not found in the auction.")

# Task 3 - End of Auction
def end_of_auction(auction_items):
    total_fee = 0
    items_sold = 0
    items_not_met_reserve = 0
    items_no_bids = 0

    print("\nEnd of Auction Results:")

    for item in auction_items:
        if item.num_bids > 0:
            if item.highest_bid >= item.reserve_price:
                item.sold = True
                items_sold += 1
                auction_fee = 0.1 * item.highest_bid
                total_fee += auction_fee
                print(f"Item {item.item_number} - Sold, Final Bid: ${item.highest_bid}, Auction Fee: ${auction_fee}")
            else:
                items_not_met_reserve += 1
                print(f"Item {item.item_number} - Did not meet reserve price, Final Bid: ${item.highest_bid}")
        else:
            items_no_bids += 1
            print(f"Item {item.item_number} - No bids received.")

    print("\nSummary:")
    print(f"Total Auction Company Fee: ${total_fee}")
    print(f"Items Sold: {items_sold}")
    print(f"Items Not Meeting Reserve Price: {items_not_met_reserve}")
    print(f"Items with No Bids: {items_no_bids}")

# Function to find an item in the auction by item number
def find_item(auction_items, item_number):
    for item in auction_items:
        if item.item_number == item_number:
            return item
    return None

# Main Program
if __name__ == "__main__":
    auction_items_list = setup_auction()

    # Simulate buyer bids (Task 2)
    place_bid(auction_items_list, buyer_number="123")
    place_bid(auction_items_list, buyer_number="456")

    # Display auction results (Task 3)
    end_of_auction(auction_items_list)
