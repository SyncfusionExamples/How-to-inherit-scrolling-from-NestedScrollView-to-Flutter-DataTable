# How to inherit scrolling from NestedScrollView to Flutter DataTable (SfDataGrid)?

In this article, we will show you how to inherit scrolling from NestedScrollView to [Flutter DataTable](https://www.syncfusion.com/flutter-widgets/flutter-datagrid).

Initialize the [SfDataGrid](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid-class.html) with the necessary properties. The [NestedScrollView](https://api.flutter.dev/flutter/widgets/NestedScrollView-class.html) enables a SliverAppBar (or other sliver widgets) to remain fixed while allowing the inner content, such as SfDataGrid, to scroll independently. To avoid conflicts with the scrolling mechanism, [NeverScrollableScrollPhysics](https://api.flutter.dev/flutter/widgets/NeverScrollableScrollPhysics-class.html) is applied to disable DataGrid’s internal scrolling, ensuring that the parent widget controls it. [SliverOverlapAbsorber](https://api.flutter.dev/flutter/widgets/SliverOverlapAbsorber-class.html) and [SliverOverlapInjector](https://api.flutter.dev/flutter/widgets/SliverOverlapInjector-class.html) maintain layout consistency, while [SliverToBoxAdapter](https://api.flutter.dev/flutter/widgets/SliverToBoxAdapter-class.html) wraps the SfDataGrid for seamless integration within the sliver structure. Additionally, setting [shrinkWrapRows](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid/shrinkWrapRows.html) to true ensures the DataGrid adjusts its height dynamically, allowing the parent widget to manage scrolling effectively.

```dart
@override
  Widget build(BuildContext context) {
    final List<String> tabs = ['Tab 1', 'Tab 2', 'Tab 3'];

    return DefaultTabController(
      length: tabs.length,
      child: Scaffold(
        body: NestedScrollView(
          headerSliverBuilder: (BuildContext context, bool innerBoxIsScrolled) {
            return [
              SliverOverlapAbsorber(
                handle:
                    NestedScrollView.sliverOverlapAbsorberHandleFor(context),
                sliver: SliverAppBar(
                  title: const Text('Syncfusion Flutter DataGrid'),
                  floating: false,
                  pinned: true,
                  expandedHeight: 40.0,
                  bottom: TabBar(
                    labelColor: Colors.black,
                    unselectedLabelColor: Colors.grey,
                    controller: _tabController,
                    isScrollable: true,
                    tabs: const [
                      Tab(child: Text('1')),
                      Tab(child: Text('2')),
                      Tab(child: Text('3')),
                    ],
                  ),
                ),
              ),
            ];
          },
          body: TabBarView(
            controller: _tabController,
            children: tabs.map((String name) {
              return SafeArea(
                top: false,
                bottom: false,
                child: Builder(
                  builder: (BuildContext context) {
                    return CustomScrollView(
                      key: PageStorageKey<String>(name),
                      slivers: [
                        SliverOverlapInjector(
                          handle:
                              NestedScrollView.sliverOverlapAbsorberHandleFor(
                                  context),
                        ),
                        SliverPadding(
                          padding: const EdgeInsets.all(8.0),
                          sliver: SliverToBoxAdapter(
                            child: buildDataGrid(),
                          ),
                        ),
                      ],
                    );
                  },
                ),
              );
            }).toList(),
          ),
        ),
      ),
    );
}
```

You can download this example on [GitHub](https://github.com/SyncfusionExamples/How-to-inherit-scrolling-from-NestedScrollView-to-Flutter-DataTable).