# Swipe Action Hints

Use this reference when rows have hidden swipe actions that users may not discover.

Sources:

- https://flutterpro.design/details/flutter-slidable-controller
- https://pub.dev/packages/flutter_slidable

## Problem

Swipe actions are invisible until users try the gesture. If delete, edit, archive, or complete actions are important, show a short one-time preview.

## Pattern

Use flutter_slidable with a SlidableController to open and close action panes programmatically.

Show the hint:

- Only for the first relevant item.
- Only after the page settles.
- Only once across app sessions.
- Briefly enough to teach, not annoy.

## Preview flow

    class TaskItem extends StatefulWidget {
      const TaskItem({
        super.key,
        required this.task,
        required this.isFirst,
      });

      final Task task;
      final bool isFirst;

      @override
      State<TaskItem> createState() => _TaskItemState();
    }

    class _TaskItemState extends State<TaskItem>
        with SingleTickerProviderStateMixin {
      late final SlidableController _controller;

      @override
      void initState() {
        super.initState();
        _controller = SlidableController(this);

        if (widget.isFirst) {
          _runPreview();
        }
      }

      @override
      void dispose() {
        _controller.dispose();
        super.dispose();
      }

      Future<void> _runPreview() async {
        await Future.delayed(const Duration(milliseconds: 500));
        if (!mounted) return;

        await _controller.openStartActionPane(
          duration: const Duration(milliseconds: 400),
        );

        await Future.delayed(const Duration(milliseconds: 900));
        if (!mounted) return;

        await _controller.close(
          duration: const Duration(milliseconds: 300),
        );
      }
    }

Persist the shown state yourself or use a small run-once helper/package so the preview does not repeat every launch.

## Review checklist

- Does the row expose an obvious non-gesture alternative for critical actions?
- Is the preview shown once, not every render?
- Does it run after the screen settles?
- Does it avoid interrupting active scrolling or editing?
- Are destructive swipe actions still confirmable or undoable when risk is high?
