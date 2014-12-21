<h1>Contact Manager</h1>

This is the 1st of 4 programming courseworks I completed (Dec 2013/Jan 2014) as part of a conversion masters in Computer Science at Birkbeck College London.

The coursework task was to implement interfaces describing a simple contact manager application. 

It was also intended that implementing the class ContactManager would serve as a demonstration of an anti-pattern ("the blob" aka "god object"), wherein too much functionality is placed into one class.

(This and the other 3 programming courseworks came after the introduction to programming with 'Java exercises' https://github.com/bbk-pij-2012-20/JavaExercises)


<h3>Implementation details</h3>

The program should implement the interfaces provided below. Classes that implement an interface should have the same name with the “Impl” suffix unless there is more than one. For example, if there is only one class that implements ContactManager, it must be called ContactManagerImpl. Be careful with this, as it may interfere with automatic tools that will analyse your code.

<h4>ContactManager</h4>

'''
import java.util.Calendar;
import java.util.List;
import java.util.Set;

/**
* A class to manage your contacts and meetings.
*/
public interface ContactManager {

/**
* Add a new meeting to be held in the future.
*
* @param contacts a list of contacts that will participate in the meeting
* @param date the date on which the meeting will take place
* @return the ID for the meeting
* @throws IllegalArgumentException if the meeting is set for a time in the past,
* of if any contact is unknown / non-existent
*/
int addFutureMeeting(Set<Contact> contacts, Calendar date);

/**
* Returns the PAST meeting with the requested ID, or null if it there is none.
*
* @param id the ID for the meeting
* @return the meeting with the requested ID, or null if it there is none.
* @throws IllegalArgumentException if there is a meeting with that ID happening in the future
1
*/
PastMeeting getPastMeeting(int id);

/**
* Returns the FUTURE meeting with the requested ID, or null if there is none.
*
* @param id the ID for the meeting
* @return the meeting with the requested ID, or null if it there is none.
* @throws IllegalArgumentException if there is a meeting with that ID happening in the past
*/
FutureMeeting getFutureMeeting(int id);

/**
* Returns the meeting with the requested ID, or null if it there is none.
*
* @param id the ID for the meeting
* @return the meeting with the requested ID, or null if it there is none.
*/
Meeting getMeeting(int id);

/**
* Returns the list of future meetings scheduled with this contact.
*
* If there are none, the returned list will be empty. Otherwise,
* the list will be chronologically sorted and will not contain any
* duplicates.
*
* @param contact one of the user’s contacts
* @return the list of future meeting(s) scheduled with this contact (maybe empty).
* @throws IllegalArgumentException if the contact does not exist
*/
List<Meeting> getFutureMeetingList(Contact contact);

/**
* Returns the list of meetings that are scheduled for, or that took
* place on, the specified date
*
* If there are none, the returned list will be empty. Otherwise,
* the list will be chronologically sorted and will not contain any
* duplicates.
*
* @param date the date
* @return the list of meetings
*/
List<Meeting> getFutureMeetingList(Calendar date);

/**
* Returns the list of past meetings in which this contact has participated.
*
* If there are none, the returned list will be empty. Otherwise,
* the list will be chronologically sorted and will not contain any
* duplicates.
*
* @param contact one of the user’s contacts
* @return the list of future meeting(s) scheduled with this contact (maybe empty).
* @throws IllegalArgumentException if the contact does not exist
*/
List<PastMeeting> getPastMeetingList(Contact contact);

/**
* Create a new record for a meeting that took place in the past.
*
* @param contacts a list of participants
* @param date the date on which the meeting took place
* @param text messages to be added about the meeting.
* @throws IllegalArgumentException if the list of contacts is
* empty, or any of the contacts does not exist
* @throws NullPointerException if any of the arguments is null
*/
void addNewPastMeeting(Set<Contact> contacts, Calendar date, String text);

/**
* Add notes to a meeting.
*
* This method is used when a future meeting takes place, and is
* then converted to a past meeting (with notes).
*
* It can be also used to add notes to a past meeting at a later date.
*
* @param id the ID of the meeting
* @param text messages to be added about the meeting.
* @throws IllegalArgumentException if the meeting does not exist
* @throws IllegalStateException if the meeting is set for a date in the future
* @throws NullPointerException if the notes are null
*/
void addMeetingNotes(int id, String text);

/**
* Create a new contact with the specified name and notes.
*
* @param name the name of the contact.
* @param notes notes to be added about the contact.
* @throws NullPointerException if the name or the notes are null
*/
void addNewContact(String name, String notes);

/**
* Returns a list containing the contacts that correspond to the IDs.
*
* @param ids an arbitrary number of contact IDs
* @return a list containing the contacts that correspond to the IDs.
* @throws IllegalArgumentException if any of the IDs does not correspond to a real contact
*/
Set<Contact> getContacts(int... ids);

/**
* Returns a list with the contacts whose name contains that string.
*
* @param name the string to search for
* @return a list with the contacts whose name contains that string.
* @throws NullPointerException if the parameter is null
*/
Set<Contact> getContacts(String name);

/**
* Save all data to disk.
*
* This method must be executed when the program is
* closed and when/if the user requests it.
*/
void flush();
}
'''

<h4>Contact</h4>
/**
* A contact is a person we are making business with or may do in the future.
*
* Contacts have an ID (unique), a name (probably unique, but maybe
* not), and notes that the user may want to save about them.
*/
public interface Contact {

/**
* Returns the ID of the contact.
*
* @return the ID of the contact.
*/
int getId();

/**
* Returns the name of the contact.
*
* @return the name of the contact.
*/
String getName();

/**
* Returns our notes about the contact, if any.
*
* If we have not written anything about the contact, the empty
* string is returned.
*
* @return a string with notes about the contact, maybe empty.
*/
String getNotes();

/**
* Add notes about the contact.
*
* @param note the notes to be added
*/
void addNotes(String note);

}

<h4>Meeting</h4>
'''
import java.util.Calendar;
import java.util.Set;

/**
* A class to represent meetings
*
* Meetings have unique IDs, scheduled date and a list of participating contacts
*/
public interface Meeting {

/**
* Returns the id of the meeting.
*
* @return the id of the meeting.
*/
int getId();

/**
* Return the date of the meeting.
*
* @return the date of the meeting.
*/
Calendar getDate();

/**
* Return the details of people that attended the meeting.
*
* The list contains a minimum of one contact (if there were
* just two people: the user and the contact) and may contain an
* arbitraty number of them.
*
* @return the details of people that attended the meeting.
*/
Set<Contact> getContacts();
}
'''
<h4>PastMeeting</h4>
'''
/**
* A meeting that was held in the past.
*
* It includes your notes about what happened and what was agreed.
*/
public interface PastMeeting extends Meeting {

/**
* Returns the notes from the meeting.
*
* If there are no notes, the empty string is returned.
*
* @return the notes from the meeting.
*/
String getNotes();
}
'''

<h4>FutureMeeting</h4>
'''
/**
* A meeting to be held in the future
*/
public interface FutureMeeting extends Meeting{
// No methods here, this is just a naming interface

// (i.e. only necessary for type checking and/or downcasting)
}
'''