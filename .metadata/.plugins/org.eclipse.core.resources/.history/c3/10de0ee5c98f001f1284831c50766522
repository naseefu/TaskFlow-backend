package com.task.entity;

import java.time.Duration;
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.List;

import jakarta.persistence.CascadeType;
import jakarta.persistence.Entity;
import jakarta.persistence.FetchType;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;
import jakarta.persistence.JoinColumn;
import jakarta.persistence.ManyToOne;
import jakarta.persistence.OneToMany;
import jakarta.persistence.Table;
import jakarta.validation.constraints.NotBlank;

@Entity
@Table(name="task")
public class Task {
	
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;
	
	@ManyToOne(fetch = FetchType.EAGER )
	@JoinColumn(name = "user_id")
	private User user;
	
	@ManyToOne(fetch = FetchType.EAGER )
	@JoinColumn(name = "team_id")
	private Team team;
	
	@ManyToOne(fetch = FetchType.EAGER )
	@JoinColumn(name = "project_id")
	private Project project;
	
	@NotBlank(message="Task name is required")
	private String taskname;
	
	@NotBlank(message="Task description is required")
	private String taskdescription;
	
	private Long assignedto;
	
	private String status;
	
	private LocalDateTime startdate;
	
	private LocalDateTime enddate;
	
	private String priority;
	
	private LocalDateTime completedat;
	
	private String jobtype;

	private String duration;
	
	@OneToMany(mappedBy = "task", fetch = FetchType.LAZY, cascade = CascadeType.ALL)
	private List<TaskMember> taskmembers = new ArrayList<>();

	public String getDuration() {
	    return calculateDuration(LocalDateTime.now(), this.enddate);
	}
	

	private String calculateDuration(LocalDateTime today, LocalDateTime enddate) {
	    Duration duration = Duration.between(today, enddate);
	    

	    long days = duration.toDays();
	    long hours = duration.toHoursPart();
	    long minutes = duration.toMinutesPart();
	    
	    
	    if(today.isAfter(enddate)) {
	    	return "Expired";
	    }
	    
	    StringBuilder result = new StringBuilder();

	    if (days > 0) {
	        result.append(days).append(days == 1 ? " day " : " days ");
	        if (hours > 0 || minutes > 0) {
	            if (hours > 0) {
	                result.append(hours).append(hours == 1 ? " h " : " h ");
	            }
	            else if (minutes > 0) {
	                result.append(minutes).append(minutes == 1 ? " min" : " min");
	            }
	        }
	    } else {
	        if (hours > 0) {
	            result.append(hours).append(hours == 1 ? " h " : " h ");
	        }
	        if (minutes > 0) {
	            result.append(minutes).append(minutes == 1 ? " min" : " min");
	        }
	    }
	    if (days == 0 && hours == 0 && minutes == 0) {
	        return "less than a minute";
	    }
	    
	    return result.toString().trim();
	}

	
	public String getJobtype() {
		return jobtype;
	}

	public void setJobtype(String jobtype) {
		this.jobtype = jobtype;
	}

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public User getUser() {
		return user;
	}

	public void setUser(User user) {
		this.user = user;
	}

	public Team getTeam() {
		return team;
	}

	public void setTeam(Team team) {
		this.team = team;
	}

	public Project getProject() {
		return project;
	}

	public void setProject(Project project) {
		this.project = project;
	}

	public String getTaskname() {
		return taskname;
	}

	public void setTaskname(String taskname) {
		this.taskname = taskname;
	}

	public String getTaskdescription() {
		return taskdescription;
	}

	public void setTaskdescription(String taskdescription) {
		this.taskdescription = taskdescription;
	}

	public Long getAssignedto() {
		return assignedto;
	}

	public void setAssignedto(Long assignedto) {
		this.assignedto = assignedto;
	}

	public String getStatus() {
		return status;
	}

	public void setStatus(String status) {
		this.status = status;
	}

	public LocalDateTime getStartdate() {
		return startdate;
	}

	public void setStartdate(LocalDateTime startdate) {
		this.startdate = startdate;
	}

	public LocalDateTime getEnddate() {
		return enddate;
	}

	public void setEnddate(LocalDateTime enddate) {
		this.enddate = enddate;
	}

	public String getPriority() {
		return priority;
	}

	public void setPriority(String priority) {
		this.priority = priority;
	}

	public LocalDateTime getCompletedat() {
		return completedat;
	}

	public void setCompletedat(LocalDateTime completedat) {
		this.completedat = completedat;
	}

	
	
	

	

}
