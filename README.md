
# TASK 8: GROUPBY AND AGGREGATION

# Average metrics by category
category_summary = df.groupby("category_name").agg(
    record_count=("video_id", "count"),
    average_views=("views", "mean"),
    average_likes=("likes", "mean"),
    average_engagement=("engagement_score", "mean")
).sort_values("average_views", ascending=False)

print("Category summary:")
display(category_summary)

# Average metrics by country
country_summary = df.groupby("country").agg(
    record_count=("video_id", "count"),
    average_views=("views", "mean"),
    average_likes=("likes", "mean"),
    average_engagement=("engagement_score", "mean")
)

print("\nCountry summary:")
display(country_summary)

# Category distribution: counts and percentages
category_counts = df["category_name"].value_counts()

category_percentages = (
    df["category_name"].value_counts(normalize=True) * 100
)

print("\nCategory counts:")
display(category_counts)

print("\nCategory percentages:")
display(category_percentages.round(2))

# Highest-viewed publishing day
df["publish_day"] = df["publish_time"].dt.day_name()
df["publish_hour"] = df["publish_time"].dt.hour

print("\nAverage views by publishing day:")
display(
    df.groupby("publish_day")["views"]
      .mean()
      .sort_values(ascending=False)
)

print("\nAverage views by publishing hour:")
display(
    df.groupby("publish_hour")["views"]
      .mean()
      .sort_values(ascending=False)
)


# TASK 9: TIME TO TREND AND TRENDING LIFESPAN

# Remove timezone information for date subtraction
df["publish_time"] = df["publish_time"].dt.tz_localize(None)

df["days_to_trend"] = (
    df["trending_date"] - df["publish_time"]
).dt.total_seconds() / 86400

print("\nDays to trend statistics:")
display(df["days_to_trend"].describe())

# Trending lifespan: distinct days for each video in each country
lifespan = df.groupby(
    ["video_id", "country"]
).agg(
    trending_days=("trending_date", "nunique"),
    title=("title", "first"),
    category_name=("category_name", "first")
).reset_index()

print("\n10 longest-lasting videos:")
display(
    lifespan.sort_values(
        "trending_days", ascending=False
    ).head(10)
)

print("\nAverage trending lifespan by category:")
display(
    lifespan.groupby("category_name")["trending_days"]
    .mean()
    .sort_values(ascending=False)
)


# TASK 10: CONCATENATION AND MERGING

# Per-video summary
per_video_summary = df.groupby(
    ["channel_title", "video_id"]
).agg(
    video_views=("views", "max")
).reset_index()

# Per-channel summary
per_channel_summary = per_video_summary.groupby(
    "channel_title"
).agg(
    trending_videos=("video_id", "nunique"),
    total_views=("video_views", "sum")
).reset_index()

# Merge both summary tables
channel_report = per_video_summary.merge(
    per_channel_summary,
    on="channel_title",
    how="left"
)

print("\nChannel report:")
display(
    channel_report.drop_duplicates("channel_title")
    .sort_values("total_views", ascending=False)
    .head(20)
)

# Save final combined dataset
df.to_csv("trendpulse_part2_cleaned.csv", index=False)

print("\nTasks 8, 9 and 10 completed!")
print("Final cleaned dataset:", df.shape)
