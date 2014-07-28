---
layout: post
title: "Single Responsibility Principle: A SOLID Week"
date: 2014-07-28 08:02:54 -0500
comments: true
categories: [solid, ruby, intermediate, object design]
---

The Single Responsibility is often written as “A class should have only a single responsibility.” Well that’s a bit general. A definition like that is a bit hard to reason about. It’s not very actionable either. There’s no sense of how to define responsibilities. Nothing that really gives you any hints on where to draw boundaries. Taken to the extreme, it actually lead to overly complex systems. There is such a thing as too much decomposition.

I think there’s a better definition of the Single Responsibility Principle and that is: “Design classes so there should never be more than one reason for a class to change.” Now that’s actionable advice. And, it’s defined within the context of the change that is bound to happen.

Let’s go into a practical example of how I might apply the Single Responsibility Principle.

Say we need a new feature that generates a report and sends it to Jill in Finance. Jill is really nice and always puts on a new pot of coffee on when we run out so we’ve decided to prioritize the feature for her. A basic approach could be to do the following.

```ruby
class Reporter
  def send_report
    users = User.where("last_logged_in_at >= ?", 1.week.ago)

    users.each do |user|
      message = "Id: #{user.id}\n"
      message += "Username: #{user.username}\n"
      message += "Last Login: #{user.last_logged_in_at.strftime("%D")}\n"
      message += "\n"
    end

    Mail.deliver do
      from "jjbohn@gmail.com"
      to "jill@example.com"
      subject "Your report"
      body message
    end
  end
end
```


Now would be a good time to ask ourselves, what is likely to change. A few likely examples are:
  * Jill gets fired and no longer works at the company so someone else needs to get the email
  * The format of the email is pretty terrible. Could I get a spreadsheet?
  * Could you send a monthly report too?
  * Could you include when the user signed up?

All of those things would change this single class. Let’s start by isolating a few concerns.

```ruby
class Reporter
  def send_report
    message = generate_message(users)

    deliver_report(message)
  end

  private

  def users
  end

  def generate_message(users)
    message = ""
    users.each do |user|
      message += "Id: #{user.id}\n"
      message += "Username: #{user.username}\n"
      message += "Last Login: #{user.last_logged_in_at.strftime("%D")}\n"
      message += "\n"
    end

    message
  end

  def deliver_report(message)
    Mail.deliver do
      from "jjbohn@gmail.com"
      to "jill@example.com"
      subject "Your report"
      body message
    end
  end
end
```


The code isn’t really any more extensible yet, but the responsibilities are starting to show themselves more.
  1. Get a list of users given a criteria
  2. Format the collection of users into a report
  3. Deliver the report

Let’s break the system down along these lines.

```ruby
class ReportDataCsvCompiler
  attr_accessor :data
  def initialize(data)
    self.data = Array(data)
  end

  def format
    heading << body
  end

  private

  def heading
    data.first.keys.join(",") << "\n"
  end

  def body
    data.each_with_object("") do |str, row|
      str << row.values.join(",") << "\n"
    end
  end
end

class User
  scope :logged_in_this_week, ->{ where("last_logged_in_at >= ?", 1.week.ago) }
end

class UserWeeklyReport
  def self.name
    "Weekly User Report"
  end

  def self.formatter
    ReportDataCsvCompiler
  end

  def self.data
    UserWeeklyReportData
  end

  def to_file
    File.write("/tmp/my_report")
  end
end

class ReportMailer
  attr_accessor :report, :recipient

  def intiialize(report:, recipient:)
    self.report = report
    self.recipient = recipient
  end

  def deliver!
    mail = Mail.new do
      from "jjbohn@gmail.com"
      to recipient
      subject report.name
    end

    mail.add_file(report.to_file)
    mail.deliver!
  end
end

class UserWeeklyReportData
  def to_data
    User.logged_in_this_week.map do |user|
      {
        id: user.id,
        username: user.username,
        last_logged_in_at: user.last_logged_in_at.strftime("%D"),
      }
    end
  end
end

# Then in you client code
ReportMailer.new(UserWeeklyReport, "jill@example.com")
```

Wow. That’s a lot more code. That said, the concerns are obvious and parts can easily be swapped out. Now that we’ve segregated responsibilities, a new feature such as uploading the report to dropbox is really easy. You just swap out the report delivery component, the `ReportMailer` in this case, with a new class and you’re all set. All the pieces become independent. They can evolve (and be tested) independently.

### Conclusion
To wrap up, how far you decompose components based on responsibility all depends on the system you’re talking about. All systems are different and what is right for one may be overly complex for another. My rule of thumb is the following. The more likely a component or set of components are to change, the more I will split said components up by responsibility. In the end, be pragmatic about this and all programming “rules”.
